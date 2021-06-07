cmake_minimum_required(VERSION 3.6)
project(ImGUI-Starter)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_VERBOSE_MAKEFILE 1)
set(APP_VERSION_MAJOR 1)
set(APP_VERSION_MINOR 0)

set(APP_TARGET ImGUI-Starter)

add_executable(${APP_TARGET} src/main.cpp)

target_compile_options(${APP_TARGET} PRIVATE -Wall)

# opengl
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS})
target_link_libraries(${APP_TARGET} PRIVATE ${OPENGL_LIBRARIES})

# glfw
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

add_subdirectory(lib/glfw EXCLUDE_FROM_ALL)
target_link_libraries(${APP_TARGET} PRIVATE glfw)
include_directories(lib/glfw/include)

# glew
add_compile_definitions(GLEW_STATIC)
add_subdirectory(lib/glew EXCLUDE_FROM_ALL)
target_link_libraries(${APP_TARGET} PRIVATE libglew_static)
include_directories(lib/glew/include)

# imgui
add_subdirectory(lib/imgui)
include_directories(lib/imgui)
target_link_libraries(${APP_TARGET} PRIVATE imgui)

# target_link_libraries(${APP_TARGET} imgui)

if (APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(OpenGL_LIBRARY OpenGL)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(COREVIDEO_LIBRARY CoreVideo)
    SET(EXTRA_LIBS ${COCOA_LIBRARY} ${OpenGL_LIBRARY} ${IOKIT_LIBRARY} ${COREVIDEO_LIBRARY})
endif (APPLE)

if (WIN32)
# nothing now
endif (WIN32)
