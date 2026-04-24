cmake_minimum_required(VERSION 3.5...3.27)

project(livox_sdk2)

# Set C++ standard
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(Threads REQUIRED)

# Compiler flags
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

# Collect all source files from sdk_core
file(GLOB_RECURSE SDK_CORE_SOURCES
    "sdk_core/*.cpp"
)

# Create the main library
add_library(livox_lidar_sdk_shared SHARED ${SDK_CORE_SOURCES})

# Include directories
target_include_directories(livox_lidar_sdk_shared PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/sdk_core>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/3rdparty>
    $<INSTALL_INTERFACE:include>
)

# Link dependencies
target_link_libraries(livox_lidar_sdk_shared
    PUBLIC Threads::Threads
)

# Install library
install(TARGETS livox_lidar_sdk_shared
    EXPORT livox_sdk2_targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

# Install headers
install(DIRECTORY include/
    DESTINATION include
)

# Install cmake config files
install(EXPORT livox_sdk2_targets
    FILE livox_sdk2Targets.cmake
    NAMESPACE livox_sdk2::
    DESTINATION lib/cmake/livox_sdk2
)

# Export for ament
ament_export_targets(livox_sdk2_targets HAS_LIBRARY_TARGET)
ament_export_libraries(livox_lidar_sdk_shared)
ament_export_include_directories(include)

ament_package()

