cmake_minimum_required(VERSION 3.5)
project(ypspur_ros)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()


if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)

find_package(builtin_interfaces REQUIRED)
find_package(diagnostic_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(trajectory_msgs REQUIRED)

set(msg_files
  "msg/DigitalOutput.msg"
  "msg/DigitalInput.msg"
  "msg/ControlMode.msg"
  "msg/JointPositionControl.msg"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  DEPENDENCIES builtin_interfaces std_msgs
  ADD_LINTER_TESTS
)
ament_export_dependencies(rosidl_default_runtime)

find_package(Boost REQUIRED COMPONENTS chrono thread atomic REQUIRED)
find_package(ypspur 1.20.0 REQUIRED)

include_directories(
  include
  ${ypspur_INCLUDE_DIRS}
)

set(dependencies
  diagnostic_msgs
  geometry_msgs
  nav_msgs
  rclcpp
  sensor_msgs
  std_msgs
  tf2
  trajectory_msgs
)

#add_executable(ypspur_ros src/ypspur_ros.cpp)
#target_link_libraries(ypspur_ros
#  ${catkin_LIBRARIES}
#  ypspur
#)
#add_dependencies(ypspur_ros ${ypspur_ros_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

#add_executable(joint_tf_publisher src/joint_tf_publisher.cpp)
#target_link_libraries(joint_tf_publisher ${catkin_LIBRARIES})
#add_dependencies(joint_tf_publisher ${ypspur_ros_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

#add_executable(joint_position_to_joint_trajectory src/joint_position_to_joint_trajectory.cpp)
#target_link_libraries(joint_position_to_joint_trajectory ${catkin_LIBRARIES})
#add_dependencies(joint_position_to_joint_trajectory ${ypspur_ros_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

#############
## Install ##
#############

#install(TARGETS
#    ypspur_ros
#    joint_tf_publisher
#    joint_position_to_joint_trajectory
#  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
#)

#############
## Testing ##
#############

if(CATKIN_ENABLE_TESTING)
  add_subdirectory(test)
  find_package(roslint REQUIRED)
  roslint_cpp()
  roslint_add_test()
endif()

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_export_include_directories(include)
ament_export_dependencies(${dependencies})
ament_package()
