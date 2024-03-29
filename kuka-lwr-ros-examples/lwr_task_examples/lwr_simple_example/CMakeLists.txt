cmake_minimum_required(VERSION 2.8.3)
project(lwr_simple_example)

find_package(catkin REQUIRED lwr_ros_client tf)


set(CMAKE_CXX_FLAGS "-O2 -O3 -std=c++11 -mtune=native -fPIC -Wall -Wno-deprecated-declarations")


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
  LIBRARIES lwr_simple_example
  CATKIN_DEPENDS lwr_ros_client
#  DEPENDS system_lib
)

###########
## Build ##
###########

set(INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

include_directories(include ${catkin_INCLUDE_DIRS})


add_library(${PROJECT_NAME} ${SRC_DIR}/simple_actions/linear_cart_action.cpp
                           ${INCLUDE_DIR}/simple_actions/linear_cart_action.h)

target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} )

install(
  TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)
install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

add_executable(client_console_node  ${SRC_DIR}/client_console_node.cpp      )
target_link_libraries(client_console_node  ${catkin_LIBRARIES}              )


add_executable(client_action_node  ${SRC_DIR}/client_action_node.cpp	)
target_link_libraries(client_action_node  ${PROJECT_NAME} ${catkin_LIBRARIES})


