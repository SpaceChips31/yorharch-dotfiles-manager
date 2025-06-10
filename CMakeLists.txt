cmake_minimum_required(VERSION 3.25)
project(yorharch VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}/build)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/build)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_SOURCE_DIR}/build)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_SOURCE_DIR}/build)

# === OpenGL ===
find_package(OpenGL REQUIRED)

# === GLFW ===
add_subdirectory(${CMAKE_SOURCE_DIR}/external/glfw)

set(GLFW_DIR ${CMAKE_SOURCE_DIR}/external/GLFW)

# === ImGui ===

set(IMGUI_DIR ${CMAKE_SOURCE_DIR}/external/imgui)

add_library(imgui STATIC
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

target_include_directories(imgui PUBLIC
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
    ${GLFW_DIR}/include
)

target_link_libraries(imgui PUBLIC glfw)

# === Main ===

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS ${CMAKE_SOURCE_DIR}/src/*.cpp)
file(GLOB_RECURSE HEADERS CONFIGURE_DEPENDS ${CMAKE_SOURCE_DIR}/include/*.hpp)

add_executable(yorharch ${SOURCES} ${HEADERS})

target_include_directories(yorharch PRIVATE ${CMAKE_SOURCE_DIR}/include)

target_link_libraries(yorharch PRIVATE
    imgui
    glfw
    OpenGL::GL
    ${CMAKE_DL_LIBS}
)