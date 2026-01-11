cmake_minimum_required(VERSION 4.1)
project(vulkan1)

set(CMAKE_CXX_STANDARD 26)

find_package(Vulkan REQUIRED)
find_package(glfw3 REQUIRED)
find_package(glm REQUIRED)

find_program(GLSLC glslc)

set(VERT_SHADER shaders/shader.vert)
set(FRAG_SHADER shaders/shader.frag)
set(VERT_SPV ${CMAKE_CURRENT_BINARY_DIR}/shader.vert.spv)
set(FRAG_SPV ${CMAKE_CURRENT_BINARY_DIR}/shader.frag.spv)

add_custom_command(
        OUTPUT ${VERT_SPV}
        COMMAND ${GLSLC} ${CMAKE_CURRENT_SOURCE_DIR}/${VERT_SHADER} -o ${VERT_SPV}
        DEPENDS ${VERT_SHADER}
)

add_custom_command(
        OUTPUT ${FRAG_SPV}
        COMMAND ${GLSLC} ${CMAKE_CURRENT_SOURCE_DIR}/${FRAG_SHADER} -o ${FRAG_SPV}
        DEPENDS ${FRAG_SHADER}
)

add_executable(vulkan1 main.cpp ${VERT_SPV} ${FRAG_SPV})

target_include_directories(vulkan1 PRIVATE ${Vulkan_INCLUDE_DIRS} ${GLFW_INCLUDE_DIRS} ${GLM_INCLUDE_DIRS})
target_link_libraries(vulkan1 PRIVATE Vulkan::Vulkan glfw)

if(APPLE)
    target_compile_definitions(vulkan1 PRIVATE VK_USE_PLATFORM_METAL_EXT)
endif()
