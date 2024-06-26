# Find libtorch
find_package(Torch REQUIRED)

# Specify the executable
set(SOURCE_FILE tensordual_test.cpp)
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
add_executable(dualtest ${SOURCE_FILE})

# Include GTest
find_package(GTest REQUIRED)
include_directories(src/cpp ${GTEST_INCLUDE_DIRS})

# Link GTest
target_link_libraries(dualtest ${GTEST_LIBRARIES} ${GTEST_MAIN_LIBRARIES})

# Link libtorch
target_link_libraries(dualtest "${TORCH_LIBRARIES}")
set_property(TARGET dualtest PROPERTY CXX_STANDARD 17)
add_test(NAME DualTest COMMAND ${CMAKE_BINARY_DIR}/bin/dualtest)

# Ensure the test executable is built when running 'make test'
add_custom_target(run_tests
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target dualtest
    COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
)


