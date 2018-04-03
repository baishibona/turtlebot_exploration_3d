cmake_minimum_required(VERSION 2.8.3)
project(turtlebot_exploration_3d)

set(octomap_DIR "/opt/ros/indigo/share/octomap")
set(CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS} -O3")

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    message("OpenMP enabled")
endif()


## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  octomap_msgs
  octomap_ros
  pcl_ros
  roscpp
  std_msgs
  move_base_msgs
  actionlib
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)


###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
 INCLUDE_DIRS include
#  LIBRARIES turtlebot_exploration_3d
 # CATKIN_DEPENDS geometry_msgs octomap_msgs octomap_ros octomap_server pcl_ros roscpp std_msgs actionlib move_base_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  $(OCTOMAP_INCLUDE_DIRS)
  include/
)

## Declare a cpp library
add_library(navigation                      src/navigation_utils.cpp)

## Declare a cpp executable
add_executable(turtlebot_exploration_3d     src/turtlebot_exploration_3d.cpp src/gpregressor.cpp  src/covMaterniso3.cpp)
add_executable(base                         src/turtlebot_exploration_3d_base.cpp )


target_link_libraries(turtlebot_exploration_3d
  ${catkin_LIBRARIES}
  ${OCTOMAP_LIBRARIES}
  navigation
)

target_link_libraries(base
  ${catkin_LIBRARIES}
  ${OCTOMAP_LIBRARIES}
  navigation
)



#############
## Install ##
#############
install(TARGETS turtlebot_exploration_3d navigation
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch/
)