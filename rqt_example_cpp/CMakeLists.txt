cmake_minimum_required(VERSION 3.5)
project(rqt_example_cpp)

# Add support for C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -fPIC)
endif()

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(ament_cmake REQUIRED)
find_package(rqt_gui REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(rqt_gui_cpp REQUIRED)
find_package(qt_gui_cpp REQUIRED)

set(rqt_example_cpp_SRCS
  src/rqt_example_cpp/my_plugin.cpp)

set(rqt_example_cpp_HDRS
  include/rqt_example_cpp/my_plugin.h)

set(rqt_example_cpp_UIS
  src/rqt_example_cpp/my_plugin.ui)

set(rqt_example_cpp_INCLUDE_DIRS
  include
  ${ament_cmake_INCLUDE_DIRS}
  ${rqt_gui_INCLUDE_DIRS}
  ${std_msgs_INCLUDE_DIRS}
  ${rclcpp_INCLUDE_DIRS}
  ${Qt5Widgets_INCLUDE_DIRS}
  ${rqt_gui_cpp_INCLUDE_DIRS}
  ${qt_gui_cpp_INCLUDE_DIRS}
  )

qt5_wrap_cpp(rqt_example_cpp_MOCS ${rqt_example_cpp_HDRS})

qt5_wrap_ui(rqt_example_cpp_UIS_H ${rqt_example_cpp_UIS})

include_directories(${rqt_example_cpp_INCLUDE_DIRS})

add_library(${PROJECT_NAME}
  ${rqt_example_cpp_SRCS}
  ${rqt_example_cpp_MOCS}
  ${rqt_example_cpp_UIS_H})


target_link_libraries(
  ${PROJECT_NAME}
  ${qt_gui_cpp_LIBRARIES}
  ${rqt_gui_cpp_LIBRARIES}
  Qt5::Widgets
  ${ament_LIBRARIES}
)

install(FILES plugin.xml
  DESTINATION share/${PROJECT_NAME}
)

install(
  DIRECTORY include/
  DESTINATION include
)

# set(LIBRARY_DIRS
  # ${ament_cmake_LIBRARIES}
  # ${rqt_gui_LIBRARIES}
  # ${std_msgs_LIBRARIES} ${rclcpp_LIBRARIES} ${Qt5Widgets_LIBRARIES}
  # ${rqt_gui_cpp_LIBRARIES}
  # ${qt_gui_cpp_LIBRARIES})

# set(LIBS
  # ${ament_cmake_LIBRARIES}
  # ${rqt_gui_LIBRARIES}
  # ${std_msgs_LIBRARIES}
  # ${rclcpp_LIBRARIES}
  # ${Qt5Widgets_LIBRARIES}
  # ${rqt_gui_cpp_LIBRARIES}
  # ${qt_gui_cpp_LIBRARIES})
#

# ament_export_dependencies(ament_cmake)
# ament_export_dependencies(rqt_gui)
# ament_export_dependencies(std_msgs)
# ament_export_dependencies(rclcpp)
# ament_export_dependencies(Qt5Widgets)
# ament_export_dependencies(rqt_gui_cpp)
# ament_export_dependencies(qt_gui_cpp)
# ament_export_include_directories(${INCLUDE_DIRS})
# ament_export_libraries(${PROJECT_NAME} ${LIBS})

install(PROGRAMS scripts/rqt_example_cpp
  DESTINATION lib/${PROJECT_NAME}
)
ament_package()
