cmake_minimum_required(VERSION 2.8.3)
project(cob_gazebo_ros_control)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS
  roscpp
  control_toolbox
  controller_manager
  hardware_interface 
  transmission_interface
  pluginlib
  joint_limits_interface
  urdf
  angles
  gazebo_ros_control
)

# Depend on system install of Gazebo 
find_package(gazebo REQUIRED)

catkin_package(
  CATKIN_DEPENDS
    roscpp
    controller_manager
    pluginlib
    transmission_interface
    gazebo_ros_control
  INCLUDE_DIRS include
  LIBRARIES multi_hw_interface_gazebo_ros_control multi_hw_interface_robot_hw_sim
  DEPENDS gazebo
)

#link_directories(${GAZEBO_LIBRARY_DIRS})
include_directories(include
  ${Boost_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
  ${GAZEBO_INCLUDE_DIRS}
)

## Libraries
add_library(multi_hw_interface_gazebo_ros_control src/multi_hw_interface_gazebo_ros_control_plugin.cpp)
target_link_libraries(multi_hw_interface_gazebo_ros_control ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES})

add_library(multi_hw_interface_robot_hw_sim src/multi_hw_interface_robot_hw_sim.cpp)
target_link_libraries(multi_hw_interface_robot_hw_sim ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES})

## Install
install(TARGETS multi_hw_interface_gazebo_ros_control multi_hw_interface_robot_hw_sim
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  )

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  )

install(FILES cob_gazebo_ros_control_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  )
