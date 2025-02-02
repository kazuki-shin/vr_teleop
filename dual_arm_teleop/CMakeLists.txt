cmake_minimum_required(VERSION 2.8.3)
project(dual_arm_teleop)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)
set(CMAKE_BUILD_TYPE Debug)

find_package(Eigen3 REQUIRED)

# Eigen 3.2 (Wily) only provides EIGEN3_INCLUDE_DIR, not EIGEN3_INCLUDE_DIRS
if(NOT EIGEN3_INCLUDE_DIRS)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
endif()

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  moveit_core
  moveit_ros_planning
  moveit_ros_planning_interface
  # moveit_visual_tools
  pluginlib
  roscpp
  victor_hardware_interface
  vive_msgs
  message_generation
  std_msgs
  eigen_conversions
  tf_conversions
)

#  MESSAGES -------------------------
add_service_files(
  FILES
  SetEnabled.srv
  )
generate_messages(
  DEPENDENCIES
  std_msgs
  )

# ---------------------------------------
catkin_package(
  INCLUDE_DIRS
    ${THIS_PACKAGE_INCLUDE_DIRS}
    include/${PROJECT_NAME}
  CATKIN_DEPENDS
    moveit_core
#    moveit_visual_tools
    moveit_ros_planning_interface
    message_runtime
  DEPENDS
    EIGEN3
)



include_directories(SYSTEM ${Boost_INCLUDE_DIR} ${EIGEN3_INCLUDE_DIRS}, include/${PROJECT_NAME})
include_directories(SYSTEM
# include
  ${catkin_INCLUDE_DIRS}
)


add_executable(unity_teleop_node src/unity_arm.cpp src/unity_teleop.cpp)
add_dependencies(unity_teleop_node ${PROJECT_NAME}_generate_messages_cpp)
target_link_libraries(unity_teleop_node
  ${catkin_LIBRARIES}
)

