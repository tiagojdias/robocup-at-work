cmake_minimum_required(VERSION 2.8.3)
project(mcr_task_planners)

find_package(catkin REQUIRED COMPONENTS)

catkin_package()

### TESTS
if(CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)
  roslaunch_add_file_check(ros/launch)
endif()
