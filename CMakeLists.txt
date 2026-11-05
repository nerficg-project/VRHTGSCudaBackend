CMAKE_MINIMUM_REQUIRED(VERSION 3.27.2)

project(VRHTGSRasterizer LANGUAGES CUDA CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
set(CMAKE_CUDA_STANDARD 20)

file(GLOB SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/config.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/inference.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/shared_kernels.cuh"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/config.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/config.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/kernels/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/kernels/*.cuh"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/kernels/monocular/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include/kernels/monocular/*.cuh"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/src/monocular/shared_kernels.cu"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/src/inference.cu"
    "${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/utils/*.h"
)
add_library(VRHTGSRasterizer STATIC ${SOURCES})

set(VRHTGSRasterizer_INCLUDE_DIRS
    ${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend
    PARENT_SCOPE
)
target_include_directories(VRHTGSRasterizer PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/rasterization/include
    ${CMAKE_CURRENT_SOURCE_DIR}/VRHTGSCudaBackend/utils
)
set_target_properties(VRHTGSRasterizer PROPERTIES CUDA_ARCHITECTURES "86")
set_target_properties(VRHTGSRasterizer PROPERTIES LINKER_LANGUAGE CUDA)
set_target_properties(VRHTGSRasterizer PROPERTIES FOLDER "extlibs")

target_compile_options(VRHTGSRasterizer PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:
  --generate-line-info
  --use_fast_math
>)
