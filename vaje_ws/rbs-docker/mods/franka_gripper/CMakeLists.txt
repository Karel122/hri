cmake_minimum_required(VERSION 3.4)
project(franka_gripper)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  message_generation
  control_msgs
  actionlib
  sensor_msgs
  xmlrpcpp
  actionlib_msgs
)

add_action_files(
  DIRECTORY action
  FILES Grasp.action
        Homing.action
        Stop.action
        Move.action
)

add_message_files(
  DIRECTORY msg
  FILES GraspEpsilon.msg
)

generate_messages(DEPENDENCIES actionlib_msgs)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES franka_gripper
  CATKIN_DEPENDS roscpp
                 message_runtime
                 control_msgs
                 actionlib
                 sensor_msgs
                 xmlrpcpp
                 actionlib_msgs
)
