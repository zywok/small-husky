cmake_minimum_required(VERSION 3.0.2)
project(zyw_slam)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")
## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  pcl_conversions
  pcl_ros
  cv_bridge
)
set(OpenCV_DIR /usr/local/share/OpenCV)
find_package(Sophus REQUIRED )
find_package(Pangolin REQUIRED)
find_package(PCL REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Threads)
find_package(Ceres REQUIRED)
find_package(Eigen3 REQUIRED)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")

catkin_package(
 CATKIN_DEPENDS roscpp rospy cv_bridge std_msgs message_runtime
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
# include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${Sophus_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${Pangolin_INCLUDE_DIRS}
)

link_directories(${OpenCV_LIBRARY_DIRS})

add_executable(bag2pcd2range src/bag2pcd2range.cpp src/common.h)
target_link_libraries(bag2pcd2range ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenCV_LIBS})

add_executable(pcdSampling src/pcdSampling.cpp src/common.h)
target_link_libraries(pcdSampling ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenCV_LIBS})

add_executable(Ptbag2pcd src/Ptbag2pcd.cpp src/common.h)
target_link_libraries(Ptbag2pcd ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenCV_LIBS})

add_executable(trajectoryICP src/trajectoryICP.cpp src/common.h)
target_link_libraries(trajectoryICP ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenCV_LIBS} ${Sophus_LIBRARIES} ${Pangolin_LIBRARIES})

add_executable(depthsampling src/depthsampling.cpp src/common.h)
target_link_libraries(depthsampling ${catkin_LIBRARIES} ${PCL_LIBRARIES} ${OpenCV_LIBRARIES})


