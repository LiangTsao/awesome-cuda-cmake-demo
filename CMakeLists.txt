cmake_minimum_required(VERSION 3.20)

project(MyCudaProject LANGUAGES C CXX CUDA)

# Find the CUDA toolkit
#find_package(CUDAToolkit REQUIRED)
find_package(CUDA REQUIRED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_CUDA_ARCHITECTURES 50 52 60 61 70 72 75 80 86 87 89 90 100 101 120)
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -Wno-deprecated-gpu-targets")
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    # set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -G")  # enable cuda-gdb (expensive)
endif()

set(CUDA_TOOLKIT_ROOT_DIR "C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.8")  # Windows 路径[4,7](@ref)

set(CMAKE_CUDA_COMPILER "C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.8/bin/nvcc.exe")

# Source file
# Add target for bandwidthTest
add_executable(demo main.cu)

target_compile_options(demo PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

target_compile_features(demo PRIVATE cxx_std_17 cuda_std_17)

set_target_properties(demo PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

# Link the executable against the CUDA libraries
target_link_libraries(demo PRIVATE CUDA::cudart)