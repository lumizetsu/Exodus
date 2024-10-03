cmake_minimum_required(VERSION 3.30)

project(moonveil)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

find_package(Vulkan REQUIRED)

include_directories(
    ${CMAKE_SOURCE_DIR}/vendor/GLFW/include    
    ${CMAKE_SOURCE_DIR}/vendor/glm
    ${CMAKE_SOURCE_DIR}/vendor/assimp/include
)

file(GLOB_RECURSE SRC_FILES "src/*.cpp" "src/*.h" "src/*.hpp")

add_executable(moonveil ${SRC_FILES})

target_link_libraries(
    moonveil
    Vulkan::Vulkan
    ${CMAKE_SOURCE_DIR}/vendor/GLFW/lib/glfw3.lib
    ${CMAKE_SOURCE_DIR}/vendor/assimp/lib/assimp-vc143-mt.lib
)
