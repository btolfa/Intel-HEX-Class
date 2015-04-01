cmake_minimum_required (VERSION 2.8)

project(intelhex-test)

include_directories(${GTEST_INCLUDE_DIRS} ${GMOCK_INCLUDE_DIRS})

set(TEST_SOURCES IntelHexTest.cpp)


add_executable(${PROJECT_NAME} ${TEST_SOURCES})
add_dependencies(${PROJECT_NAME} googlemock)

target_link_libraries(${PROJECT_NAME} ${GTEST_BOTH_LIBRARIES} ${GMOCK_BOTH_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})

foreach(filename ${TEST_SOURCES})
    get_filename_component(testname ${filename} NAME_WE)
    add_test(NAME ${PROJECT_NAME}.${testname}
        COMMAND ${PROJECT_NAME} --gtest_filter=${testname}*
        WORKING_DIRECTORY .) 
endforeach(filename ${SRC})

add_executable(ihex-tester ihex-test.cpp)
target_link_libraries(ihex-tester intelhex)
target_include_directories(ihex-tester PUBLIC ../../intelhex_class)