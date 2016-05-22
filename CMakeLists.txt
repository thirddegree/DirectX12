cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
cmake_policy(VERSION 3.5)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "cmake/")

project("DirectX12Examples")


include_directories("include")
include_directories("ExampleCommon/include")
file(GLOB COMMON_INC ExampleCommon/include/*.h)
file(GLOB COMMON_SRC ExampleCommon/source/*.cpp)
source_group("Header Files\\Common" FILES ${COMMON_INC})
source_group("Source Files\\Common" FILES ${COMMON_SRC})

# Define required libraries
set(ASSIMP_LIBS debug assimp optimized assimp)
set(SDL2_LIBRARY debug SDL2 SDL2main optimized SDL2 SDL2main)
set(DIRECTX12_LIBRARY debug d3d12 d3dcompiler optimized d3d12 d3dcompiler)
include_directories("${CMAKE_SOURCE_DIR}/ExampleCommon/ThirdParty/SDL2/include"
                    "${CMAKE_SOURCE_DIR}/ExampleCommon/ThirdParty/assimp/include")
link_directories(${CMAKE_SOURCE_DIR}/ExampleCommon/ThirdParty/lib/assimp/$(Configuration)
                    ${CMAKE_SOURCE_DIR}/ExampleCommon/ThirdParty/lib/SDL2/$(Configuration))



# Function for building single example
function(BuildExample EXAMPLE_NAME)
    file(GLOB EXAMPLE_INC ${COMMON_INC} include/${EXAMPLE_NAME}/*.h)
    file(GLOB EXAMPLE_SRC ${COMMON_SRC} source/*.cpp source/${EXAMPLE_NAME}/*.cpp)
    add_executable(${EXAMPLE_NAME} WIN32 ${EXAMPLE_INC} ${EXAMPLE_SRC})
    target_include_directories(${EXAMPLE_NAME} PUBLIC "include/${EXAMPLE_NAME}")
    target_link_libraries(${EXAMPLE_NAME} ${VULKAN_LIBRARY} ${SDL2_LIBRARY})
endfunction()

# Build all examples
function(BuildExamples)
    foreach(EXAMPLE ${EXAMPLES})
        BuildExample(${EXAMPLE})
    endforeach()
endfunction()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin/")

set(EXAMPLES
    triangle
    model)

BuildExamples()
