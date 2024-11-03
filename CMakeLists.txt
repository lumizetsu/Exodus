cmake_minimum_required(VERSION 3.20)

project(Exodus)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

find_package(Vulkan REQUIRED)

include_directories(
    ${CMAKE_SOURCE_DIR}/vendor/GLFW/include
    ${CMAKE_SOURCE_DIR}/vendor/glm
    ${CMAKE_SOURCE_DIR}/vendor/assimp/include
    ${Vulkan_INCLUDE_DIR}
)

file(GLOB_RECURSE SRC_FILES "src/*.cpp")

add_executable(Exodus ${SRC_FILES})

target_link_libraries(
    Exodus
    Vulkan::Vulkan
    ${CMAKE_SOURCE_DIR}/vendor/GLFW/lib/glfw3.lib
    ${CMAKE_SOURCE_DIR}/vendor/assimp/lib/assimp-vc143-mt.lib
)

find_program(GLSL_VALIDATOR glslangValidator HINTS Vulkan::glslangValidator /usr/bin /usr/local/bin ${VULKAN_SDK}/Bin/ ${VULKAN_SDK}/Bin32/)
if(NOT GLSL_VALIDATOR)
    message(FATAL_ERROR "glslangValidator not found! Please ensure Vulkan SDK is installed")
endif()

file(GLOB_RECURSE GLSL_SOURCE_FILES
    "${CMAKE_SOURCE_DIR}/shaders/*.frag"
    "${CMAKE_SOURCE_DIR}/shaders/*.vert"
    "${CMAKE_SOURCE_DIR}/shaders/*.comp"
)

foreach(GLSL ${GLSL_SOURCE_FILES})
    get_filename_component(FILE_NAME ${GLSL} NAME)
    set(SPIRV "${CMAKE_SOURCE_DIR}/shaders/${FILE_NAME}.spv")
    get_filename_component(SPIRV_NAME ${SPIRV} NAME)
    
    add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${GLSL_VALIDATOR} -V ${GLSL} -o ${SPIRV}
        DEPENDS ${GLSL}
        COMMENT "Compiling shader ${FILE_NAME} -> ${SPIRV_NAME}"
    )
    
    list(APPEND SPIRV_BINARY_FILES ${SPIRV})
endforeach()

add_custom_target(
    Shaders
    DEPENDS ${SPIRV_BINARY_FILES}
)

add_dependencies(Exodus Shaders)
