cmake_minimum_required(VERSION 3.0)

project(livox_sdk2)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    add_compile_options(-Wno-c++20-compat)
endif()

message(STATUS "main project dir: " ${PROJECT_SOURCE_DIR})

if (CMAKE_CROSSCOMPILING)
    set(THREADS_PTHREAD_ARG
        "PLEASE_FILL_OUT-FAILED_TO_RUN"
        CACHE STRING "Result from TRY_RUN" FORCE)
endif()

if (UNIX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")
endif(UNIX)

add_subdirectory(sdk_core)
add_subdirectory(samples)
