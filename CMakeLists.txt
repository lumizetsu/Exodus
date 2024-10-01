cmake_minimum_required(VERSION 3.30)

project(moonveil)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)


if(${CMAKE_CXX_COMPILER_ID} STREQUAL GNU OR
${CMAKE_CXX_COMPILER_ID} STREQUAL Clang)
    set(warnings "-Wall -Wextra -Werror")
elseif(${CMAKE_CXX_COMPILER_ID} STREQUAL MSVC)
    set(warnings "/W4 /WX /EHsc")
endif()

if(NOT CONFIGURED_ONCE)
    set(CMAKE_CXX_FLAGS "${warnings}"
        CACHE STRING "Flags used by the compiler during all build types." FORCE)
endif()

include_directories(
    
)

file(GLOB_RECURSE SRC_FILES "src/*.cpp" "src/*.h" "src/*.hpp")

add_executable(moonveil ${SRC_FILES})

target_link_libraries(
    moonveil
    d3d12.lib
    dxgi.lib
    d3dcompiler.lib
)
