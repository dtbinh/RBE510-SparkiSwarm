cmake_minimum_required(VERSION 2.8)

project(RBE_MULTI_ROBOTICS_WPI)

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
	message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

include_directories("${PROJECT_SOURCE_DIR}/src")

find_package (Threads)
find_package(OpenCV REQUIRED)

include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIBRARY})

add_executable(swarm_server ${PROJECT_SOURCE_DIR}/src/swarm_server.cpp ${PROJECT_SOURCE_DIR}/src/rbe510.cpp)
add_executable(swarm ${PROJECT_SOURCE_DIR}/src/swarm.cpp ${PROJECT_SOURCE_DIR}/src/rbe510.cpp)

target_link_libraries (swarm_server ${OpenCV_LIBS})
target_link_libraries (swarm_server ${CMAKE_THREAD_LIBS_INIT})
