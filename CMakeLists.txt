cmake_minimum_required(VERSION 3.0.2)
project(rnn_denoise)

set(CMAKE_CXX_FLAGS "-g")
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -pthread")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3")

set(PROJECT_INCLUDE_PATH "${PROJECT_SOURCE_DIR}/include")
include_directories(${PROJECT_INCLUDE_PATH})
set(PROJECT_LAYER_INCLUDE_PATH "${PROJECT_SOURCE_DIR}/include/layers")
include_directories(${PROJECT_LAYER_INCLUDE_PATH})

file(GLOB_RECURSE layers_srcs src/layers/*.c)
add_library(layers SHARED ${layers_srcs})

file(GLOB_RECURSE core_srcs src/core/*.c)
add_library(core SHARED ${core_srcs})

file(GLOB_RECURSE backends_srcs src/backends/*.c)
add_library(backends SHARED ${backends_srcs})

add_executable(main
	src/main.c
	src/mfcc.c
)

target_link_libraries(main
	layers
	core
	backends	
	-lm
)
