cmake_minimum_required(VERSION 3.10)
project(submodule-1)

enable_testing()

# Add submodule-1.1
add_subdirectory(submodule-1.1)

# Create a library for submodule-1
add_library(submodule1_lib submodule1.c)
add_executable(submodule1 submodule1.c)

# Add Google Test
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# Link submodule-1 with submodule-1.1
target_link_libraries(submodule1_lib 
    PRIVATE 
    submodule11_lib)

# Create test executable
add_executable(submodule1_tests test-submodule1.cpp)

# Link test executable with required libraries
target_link_libraries(submodule1_tests 
    PRIVATE 
    GTest::GTest 
    GTest::Main 
    pthread 
    submodule1_lib)

# Register test
add_test(NAME submodule1_test COMMAND submodule1_tests)

# Make the library target available to parent projects
set_target_properties(submodule1_lib PROPERTIES EXPORT_NAME submodule1_lib)

