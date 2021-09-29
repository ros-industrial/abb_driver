cmake_minimum_required(VERSION 3.0.2)
project(abb_driver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(catkin REQUIRED COMPONENTS industrial_robot_client simple_message)

catkin_package(
  CATKIN_DEPENDS industrial_robot_client
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)


add_executable(${PROJECT_NAME}_robot_state
  src/abb_robot_state_node.cpp
  src/abb_utils.cpp)
target_link_libraries(${PROJECT_NAME}_robot_state
  industrial_robot_client
  simple_message
  ${catkin_LIBRARIES})
set_target_properties(
  ${PROJECT_NAME}_robot_state
  PROPERTIES OUTPUT_NAME robot_state PREFIX "")
target_compile_definitions(
  ${PROJECT_NAME}_robot_state
  PRIVATE
  ${industrial_robot_client_DEFINITIONS})

add_executable(${PROJECT_NAME}_motion_download_interface
  src/abb_joint_downloader_node.cpp
  src/abb_utils.cpp)
target_link_libraries(${PROJECT_NAME}_motion_download_interface
  industrial_robot_client
  simple_message
  ${catkin_LIBRARIES})
set_target_properties(${PROJECT_NAME}_motion_download_interface
  PROPERTIES OUTPUT_NAME motion_download_interface PREFIX "")
target_compile_definitions(
  ${PROJECT_NAME}_motion_download_interface
  PRIVATE
  ${industrial_robot_client_DEFINITIONS})

install(
  TARGETS
    ${PROJECT_NAME}_robot_state
    ${PROJECT_NAME}_motion_download_interface
  DESTINATION
    ${CATKIN_PACKAGE_BIN_DESTINATION}
)

foreach(dir launch rapid)
  install(
    DIRECTORY ${dir}/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach()
