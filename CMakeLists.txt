cmake_minimum_required(VERSION 3.10)
project(OpenGLSkeleton VERSION 1.0)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Specify the directories for includes and libs
include_directories(include)
include_directories(external/glfw/include)
include_directories(external/glew/include)

# Set the output directories for the compiled libraries and binaries
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Find OpenGL
find_package(OpenGL REQUIRED)

# Add source files
file(GLOB_RECURSE SOURCES "src/*.cpp")

# Add GLEW source file
set(GLEW_SOURCES external/glew/src/glew.c)

# Add executable
add_executable(OpenGLSkeleton ${SOURCES} ${GLEW_SOURCES})

# Link GLFW library
add_library(glfw3 STATIC IMPORTED)
set_target_properties(glfw3 PROPERTIES
    IMPORTED_LOCATION "${CMAKE_SOURCE_DIR}/external/glfw/lib/libglfw3.a"
    INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_SOURCE_DIR}/external/glfw/include"
)

# Link macOS frameworks
if(APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(CORE_FOUNDATION_LIBRARY CoreFoundation)
    find_library(CORE_VIDEO_LIBRARY CoreVideo)
    mark_as_advanced(COCOA_LIBRARY IOKIT_LIBRARY CORE_FOUNDATION_LIBRARY CORE_VIDEO_LIBRARY)
    target_link_libraries(OpenGLSkeleton PRIVATE ${COCOA_LIBRARY} ${IOKIT_LIBRARY} ${CORE_FOUNDATION_LIBRARY} ${CORE_VIDEO_LIBRARY})
endif()

# Link libraries to the executable
target_link_libraries(OpenGLSkeleton PRIVATE ${OPENGL_LIBRARIES} glfw3)
