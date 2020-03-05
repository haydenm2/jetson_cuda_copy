cmake_minimum_required(VERSION 3.1)
project(jetson_cuda_copy)

## Add support for C++11, supported in ROS Kinetic and newer
set(CMAKE_CXX_STANDARD 11)
if (NOT CMAKE_BUILD_TYPE)
    # Options: Debug, Release, MinSizeRel, RelWithDebInfo
    message(STATUS "No build type selected, default to Release")
    set(CMAKE_BUILD_TYPE "Release")
endif()

find_package(CUDA REQUIRED)
include_directories("${CUDA_INCLUDE_DIRS}")

SET(CUDA_NVCC_FLAGS "-arch-sm_13" CACHE STRING "nvcc flags" FORCE)
SET(CUDA_VERBOSE_BUILD ON CACHE BOOL "nvcc verbose" FORCE)

find_package(OpenCV 4)
find_package(OpenCV REQUIRED)
message(STATUS "OpenCV Version: ${OpenCV_VERSION} (${OpenCV_DIR})")
if(OpenCV_VERSION VERSION_LESS 3.0.0)
  message(FATAL_ERROR "OpenCV version must be at least 3")
endif()

if(OpenCV_CUDA_VERSION)
message(STATUS "+++ CUDA-enabled OpenCV found, CUDA Version: " ${OpenCV_CUDA_VERSION})
add_definitions(-DOPENCV_CUDA)
else()
string(ASCII 27 ESC)
message(STATUS "${ESC}[1;31m+++${ESC}[0m" " CUDA-enabled OpenCV not found")
endif()


## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(${catkin_INCLUDE_DIRS})

add_executable(${PROJECT_NAME}
  jetson_cuda_copy.cpp)


## Specify libraries to link a library or executable target against
target_link_libraries(${PROJECT_NAME} 
  ${catkin_LIBRARIES} 
  ${OpenCV_LIBS}
  cudart)

