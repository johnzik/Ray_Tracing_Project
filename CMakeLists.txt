cmake_minimum_required(VERSION 3.14)

find_program(CPP_PATH g++)
if(CPP_PATH)
    set(CMAKE_CXX_COMPILER ${CPP_PATH})
else()
    message(WARNING "g++ not found! Using default compiler.")
endif()

project(RAY_TRACING_PROJECT LANGUAGES CXX)

# Use C++17 or newer
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)

# Set output folders
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib) # For *.a or *.lib
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib) # For *.so or *.dll
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin) # For executables

# Include folders
include_directories(
    ${PROJECT_SOURCE_DIR}/inc
)

# STATIC lib from src files
file(GLOB_RECURSE SRC_FILES src/*.cpp)
add_library(RAY_TRACING_PROJECT_lib STATIC
    ${SRC_FILES}
)

# Add exes automatically
foreach(SRC_FILE ${SRC_FILES})
    get_filename_component(TEST_NAME ${SRC_FILE} NAME_WE)

    add_executable(${TEST_NAME} ${SRC_FILES})

    target_link_libraries(${TEST_NAME} RAY_TRACING_PROJECT_lib)
endforeach()