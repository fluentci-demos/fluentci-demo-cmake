cmake_minimum_required(VERSION 3.5)

# Set the project name
project(boost_unit_test)

include(cmake/CPM.cmake)

CPMAddPackage(
    NAME Boost
    VERSION 1.84.0
    URL https://github.com/boostorg/boost/releases/download/boost-1.84.0/boost-1.84.0.tar.xz
    URL_HASH SHA256=2e64e5d79a738d0fa6fb546c6e5c2bd28f88d268a2a080546f74e5ff98f29d0e
    OPTIONS "BOOST_ENABLE_CMAKE ON" "BOOST_INCLUDE_LIBRARIES test" # Note the escapes!
)

find_package(Boost 1.84 REQUIRED COMPONENTS unit_test_framework)

# Add an library for the example classes
add_library(example_boost_unit_test
    Reverse.cpp
    Palindrome.cpp
)

target_include_directories(example_boost_unit_test
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(example_boost_unit_test
    PUBLIC
    Boost::headers
)

# ############################################
# Unit tests

# enable CTest testing
enable_testing()

# Add a testing executable
add_executable(unit_tests unit_tests.cpp)

target_link_libraries(unit_tests
    example_boost_unit_test
    Boost::unit_test_framework
)

target_compile_definitions(unit_tests
    PRIVATE
    BOOST_TEST_DYN_LINK
)

add_test(test_all unit_tests)
