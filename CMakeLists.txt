cmake_minimum_required(VERSION 3.21)
project(pb-cpp-program-template CXX)


add_executable(pb-cpp-program-template 
    src/main.cpp
)

if (UNIX)
    set(CMAKE_CXX_FLAGS "-fpermissive -Wno-write-strings")
endif()

if (WIN32)
    target_compile_definitions(pb-cpp-program-template PRIVATE _CRT_SECURE_NO_WARNINGS)
endif()

set_target_properties(pb-cpp-program-template PROPERTIES 
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
    POSITION_INDEPENDENT_CODE ON
)


#Bring the headers, plugin include, algorithm include
target_include_directories(pb-cpp-program-template PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include )

# only do the testing targets if we are doing this project.  this way
# other projects won't get this stuff.
if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    include(CTest)
    include(FetchContent)

    FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG        release-1.12.1
    )
    FetchContent_MakeAvailable(googletest)
    add_library(GTest::GTest INTERFACE IMPORTED)
    target_link_libraries(GTest::GTest INTERFACE gtest_main)


    add_executable(pb-cpp-program-template-test 
        test/DummyTest.cpp
        )

    target_link_libraries(pb-cpp-program-template-test PRIVATE
        GTest::GTest
        )

    set_target_properties(pb-cpp-program-template-test PROPERTIES 
        CXX_STANDARD 20
        CXX_STANDARD_REQUIRED YES
        CXX_EXTENSIONS NO
        POSITION_INDEPENDENT_CODE ON
    )

    add_test(pb-cpp-program-template-gtests pb-cpp-program-template-test)

    file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/test/resource/)

    file(TO_NATIVE_PATH ${PROJECT_SOURCE_DIR}/test/resource/ TEST_RESOURCES_SRC)
    file(TO_NATIVE_PATH ${CMAKE_CURRENT_BINARY_DIR}/test/resource/ TEST_RESOURCES_DEST)    
    #copy the resources used for tests into the build folder under test/resource.
    add_custom_command(
            TARGET pb-cpp-program-template-test POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_directory
                    ${TEST_RESOURCES_SRC}
                    ${TEST_RESOURCES_DEST})

endif()
