cmake_minimum_required(VERSION 2.8)
project(intelhex)

enable_testing()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(INTELHEX_SOURCES intelhex_class/intelhexclass.h intelhex_class/intelhexclass.cpp)
add_library(intelhex ${INTELHEX_SOURCES})

if (CMAKE_COMPILER_IS_GNUCXX)
    set_target_properties(intelhex PROPERTIES COMPILE_FLAGS "-Wall -Wextra -Wshadow -pedantic")
endif ()

# Install our targets
install(TARGETS intelhex DESTINATION lib)
install(FILES intelhex_class/intelhexclass.h DESTINATION include)

add_subdirectory(tests)