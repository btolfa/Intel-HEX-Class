cmake_minimum_required (VERSION 2.8)

project(tests)
# Этот cmake подключает тесты

find_package(Threads REQUIRED)

# Enable ExternalProject CMake module
include(ExternalProject)

# Set default ExternalProject root directory
set_directory_properties(PROPERTIES EP_PREFIX ${CMAKE_BINARY_DIR}/ThirdParty)

# Add gmock
ExternalProject_Add(
    googlemock
    SVN_REPOSITORY http://googlemock.googlecode.com/svn/trunk/
    SVN_REVISION -r 514
    TIMEOUT 10
    UPDATE_COMMAND ""
    # Force separate output paths for debug and release builds to allow easy
    # identification of correct lib in subsequent TARGET_LINK_LIBRARIES commands
    CMAKE_ARGS -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
               -DCMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG:PATH=DebugLibs
               -DCMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE:PATH=ReleaseLibs
               -DCMAKE_CXX_FLAGS=-std=c++14
    # Disable install step
    INSTALL_COMMAND ""
    # Wrap download, configure and build steps in a script to log output
    LOG_DOWNLOAD ON
    LOG_CONFIGURE ON
    LOG_BUILD ON)

# Specify include dir
ExternalProject_Get_Property(googlemock source_dir)
set(GMOCK_INCLUDE_DIRS ${source_dir}/include)
set(GTEST_INCLUDE_DIRS ${source_dir}/gtest/include)

# Specify link libraries
ExternalProject_Get_Property(googlemock binary_dir)
set (GMOCK_BOTH_LIBRARIES 
    debug ${binary_dir}/DebugLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gmock.a
    debug ${binary_dir}/DebugLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gmock_main.a
    optimized ${binary_dir}/ReleaseLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gmock.a
    optimized ${binary_dir}/ReleaseLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gmock_main.a)
set (GTEST_BOTH_LIBRARIES 
    debug ${binary_dir}/gtest/DebugLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest.a
    debug ${binary_dir}/gtest/DebugLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest_main.a
    optimized ${binary_dir}/gtest/ReleaseLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest.a
    optimized ${binary_dir}/gtest/ReleaseLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest_main.a)

add_subdirectory(intelhex)