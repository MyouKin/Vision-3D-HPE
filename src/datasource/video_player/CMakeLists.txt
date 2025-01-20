cmake_minimum_required(VERSION 3.8)
project(video_player)

## By adding -Wall and -Werror, the compiler does not ignore warnings anymore,
## enforcing cleaner code.
add_definitions(-Wall -Werror)

## Export compile commands for clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

## Find dependencies
find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()
find_package(OpenCV REQUIRED)

## Build
ament_auto_add_library(${PROJECT_NAME} SHARED
  src/video_player.cpp
)

## target
target_include_directories(${PROJECT_NAME} PUBLIC include ${OpenCV_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS})

## Install
rclcpp_components_register_node(${PROJECT_NAME}
  PLUGIN video_player::VideoPlayerNode
  EXECUTABLE ${PROJECT_NAME}_node
)

## Test
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  list(APPEND AMENT_LINT_AUTO_EXCLUDE
    ament_cmake_copyright
    ament_cmake_cpplint
    ament_cmake_uncrustify
  )
  ament_lint_auto_find_test_dependencies()
endif()

## Install to
ament_auto_package(
  INSTALL_TO_SHARE
  config
)