cmake_minimum_required(VERSION 2.8.3)
project(mcr_arm_cartesian_control)

find_package(catkin REQUIRED
  COMPONENTS
    cmake_modules
    roscpp sensor_msgs
    mcr_manipulation_utils
    brics_actuator
    tf
    urdf
)

find_package(Eigen REQUIRED)
find_package(orocos_kdl REQUIRED)

catkin_package(
  CATKIN_DEPENDS  
    mcr_manipulation_utils
    sensor_msgs
)

include_directories(
 ${PROJECT_SOURCE_DIR}/ros/include 
 ${PROJECT_SOURCE_DIR}/common/include
 ${EIGEN_INCLUDE_DIRS}
 ${catkin_INCLUDE_DIRS}
)

add_executable(arm_cartesian_control
  ros/src/arm_cartesian_control_node.cpp
  common/src/arm_cartesian_control.cpp)
    
target_link_libraries(arm_cartesian_control
  ${catkin_LIBRARIES} 
  ${orocos_kdl_LIBRARIES}  
)

add_dependencies(arm_cartesian_control 
  ${catkin_EXPORTED_TARGETS}
)

### TESTS
if(CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)

  roslaunch_add_file_check(ros/launch)
endif()


### INSTALLS
install(
  TARGETS arm_cartesian_control
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY common/include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY ros/include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY ros/launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/ros/launch
)
