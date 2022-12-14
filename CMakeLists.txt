cmake_minimum_required(VERSION 3.16)
project(EXAMPLE)
set(CMAKE_CXX_STANDARD 20)

set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_BUILD_RPATH_USE_ORIGIN TRUE)
set(CMAKE_INSTALL_RPATH $ORIGIN)

# Put all libraries in the executable folder
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

# libraries

# OPENGL
find_package(OpenGL REQUIRED)

# GLFW
set(GLFW_BUILD_DOCS     OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS    OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/thirdparty/glfw)
list(APPEND PROJECT_LINK_LIBRARYIES glfw)

# IMGUI
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/thirdparty/imgui)
list(APPEND PROJECT_LINK_LIBRARYIES imgui)

include(${CMAKE_CURRENT_SOURCE_DIR}/src/CMakeLists.txt)
list(APPEND PROJECT_SOURCES ${PROJECT_HEADERS})

add_executable(${PROJECT_NAME} ${PROJECT_SOURCES})

target_include_directories(${PROJECT_NAME} PUBLIC
        ${OPENGL_INCLUDE_DIRS}
        ${CMAKE_CURRENT_SOURCE_DIR}/src/thirdparty/imgui)
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)

target_link_libraries(${PROJECT_NAME} PUBLIC ${OPENGL_LIBRARIES} ${PROJECT_LINK_LIBRARYIES})

target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)