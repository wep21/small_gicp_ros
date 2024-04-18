cmake_minimum_required(VERSION 3.18)
project(small_gicp_ros)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Wno-unused-parameter)
endif()

set(CMAKE_CXX_STANDARD 17)

find_package(ament_cmake REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(OpenMP)
find_package(sensor_msgs REQUIRED)

include(FetchContent)
FetchContent_Declare(
  small_gicp
  GIT_REPOSITORY https://github.com/koide3/small_gicp.git
  GIT_TAG b85d2469d3456ef18a92e677a3bbfe85536135d9
)
FetchContent_MakeAvailable(small_gicp)
add_library(small_gicp::small_gicp ALIAS small_gicp)

add_executable(registration
  src/example.cpp
  src/point_cloud.cpp
)

target_link_libraries(registration PRIVATE
  small_gicp::small_gicp
  ${sensor_msgs_TARGETS}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
