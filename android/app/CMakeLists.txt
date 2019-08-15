# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)


SET(OpenCV_DIR D:/Android/OpenCV31/sdk/native/jni)
#SET(OpenCV_DIR D:/Android/OpenCV41/sdk/native/jni)
find_package(OpenCV REQUIRED)
message(STATUS "opencv found: ${OpenCV_LIBS}")

SET(LIB_SRC_DIR ../..)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${OpenCV_DIR}/include/
        ${LIB_SRC_DIR}/src/)

add_library(ar_core-lib
        SHARED
        ${LIB_SRC_DIR}/src/Utils/CvUtils.hpp
        ${LIB_SRC_DIR}/src/Recognition/Recognition.hpp
        ${LIB_SRC_DIR}/src/Tracking/TrackerEntity.hpp
        ${LIB_SRC_DIR}/src/Tracking/Tracking.hpp
        ${LIB_SRC_DIR}/src/Recognition/Recognition.cpp
        ${LIB_SRC_DIR}/src/Tracking/TrackerEntity.cpp
        ${LIB_SRC_DIR}/src/Tracking/Tracking.cpp
        ${LIB_SRC_DIR}/src/AR/Ar.cpp
        ${LIB_SRC_DIR}/src/AR/Ar.hpp
        ${LIB_SRC_DIR}/src/Recognition/BoVw.cpp
        ${LIB_SRC_DIR}/src/Recognition/BoVw.hpp
        ${LIB_SRC_DIR}/src/android.hpp
        ${LIB_SRC_DIR}/src/android.cpp
        src/main/cpp/native-lib.cpp
        )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib
        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
        ar_core-lib
        # Links the target library to the log library
        # included in the NDK.
        ${log-lib}
        ${OpenCV_LIBS}
        )