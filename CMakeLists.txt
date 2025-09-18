cmake_minimum_required(VERSION 3.12)

project(logjson)

set(CMAKE_CXX_STANDARD 20)


file(GLOB SOURCES "cpp/src/*.cpp")

add_library(logjson STATIC ${SOURCES})
target_include_directories(logjson PUBLIC "cpp/include")
target_compile_features(logjson PUBLIC cxx_std_20)


# Build test if not used as a dependency
if(${CMAKE_PROJECT_NAME} STREQUAL ${PROJECT_NAME})
    include(FetchContent)
    FetchContent_Declare(
    googletest
    URL https://github.com/google/googletest/archive/refs/tags/v1.17.0.zip
    )
    # For Windows: Prevent overriding the parent project's compiler/linker settings
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(googletest)

    enable_testing()

    file(GLOB_RECURSE TEST_SOURCES "test/cpp/*.cpp")
    add_executable(
        json_tests
        ${TEST_SOURCES}
    )
    target_link_libraries(
        json_tests
        GTest::gtest_main
        logjson
    )

    include(GoogleTest)
    gtest_discover_tests(json_tests)
endif()