cmake_minimum_required(VERSION 2.8.8)
# Cuda support variables
#caffe_option(CPU_ONLY "Build Caffe without CUDA support" ON) # TODO: rename to USE_CUDA

project (hello)

FIND_PACKAGE(Boost COMPONENTS
                system
                thread
                python REQUIRED)
find_package(PythonLibs REQUIRED)

#find_package(PythonLibs 3 REQUIRED)
#find_package(Boost REQUIRED COMPONENTS system python-py35)

# Find source files
file(GLOB SOURCES /home/dev/boost_hello/*.cpp)

set(Caffe_INCLUDE_DIRS "/home/dev/caffe/distribute/include")
list(APPEND Caffe_INCLUDE_DIRS "/root/opencv/include")
list(APPEND Caffe_INCLUDE_DIRS "/opt/conda/include")
list(APPEND Caffe_INCLUDE_DIRS "/opt/conda/include/python3.6m")
list(APPEND Caffe_INCLUDE_DIRS "/opt/conda/lib/python3.6/site-packages/numpy/core/include")



set(Caffe_DEFINITIONS "-DCPU_ONLY=True")
list(APPEND Caffe_DEFINITIONS "-DCMAKE_BUILD_TYPE=Debug")


set(Caffe_LIBRARIES "/home/dev/caffe/distribute/lib/libcaffe.so")
list(APPEND Caffe_LIBRARIES "/usr/lib/x86_64-linux-gnu/libglog.so.0")
#list(APPEND Caffe_LIBRARIES "/usr/lib/x86_64-linux-gnu/libboost_system.so.1.58.0")

set(CMAKE_CXX_STANDARD 14)
#set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,-z,defs")

include_directories(${Caffe_INCLUDE_DIRS})
include_directories(${PYTHON_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIRS})

add_definitions(${Caffe_DEFINITIONS} )

# Create shared library
add_library(${PROJECT_NAME} SHARED ${SOURCES})

#add_executable(hellocaffe main.cpp)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${Caffe_LIBRARIES} )
