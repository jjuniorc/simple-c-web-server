cmake_minimum_required(VERSION 3.10)
project(SimpleCWebServer C)

# Set C standard and compiler options
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

# Platform-specific configurations
if(UNIX)
    add_definitions(-D_POSIX_C_SOURCE=200809L)
    find_package(Threads REQUIRED)
endif()

# Configure compiler warnings
if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-Wall -Wextra -Wpedantic -Werror=implicit-function-declaration)
    add_compile_options(-Wno-unused-parameter)  # Allow unused parameters for callback functions
endif()

# Define the executable
add_executable(server
    server.c
)

# Link libraries
if(UNIX)
    target_link_libraries(server PRIVATE Threads::Threads)
    if(LINUX)
        target_link_libraries(server PRIVATE rt)  # For shm_open on Linux
    endif()
endif()

# Installation rules
install(TARGETS server
    RUNTIME DESTINATION bin
)

# Configure build types
set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose build type: Debug or Release")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release")

# Debug vs Release configurations
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_definitions(DEBUG)
    add_compile_options(-g -O0)
else()
    add_compile_options(-O2)
endif()

# Add custom target for running the server
add_custom_target(run
    COMMAND ./server
    DEPENDS server
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMENT "Running the web server from project root..."
)