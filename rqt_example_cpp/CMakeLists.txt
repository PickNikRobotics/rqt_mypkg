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
find_package(std_msgs REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rqt_gui REQUIRED)
find_package(rqt_gui_cpp REQUIRED)

set(rqt_example_cpp_SRCS
  src/rqt_example_cpp/my_plugin.cpp)

set(rqt_example_cpp_HDRS
  include/rqt_example_cpp/my_plugin.h)

set(rqt_example_cpp_UIS
  src/rqt_example_cpp/my_plugin.ui)

set(rqt_example_cpp_INCLUDE_DIRS
  include
  ${rqt_gui_INCLUDE_DIRS}
  ${std_msgs_INCLUDE_DIRS}
  ${rclcpp_INCLUDE_DIRS}
  ${Qt5Widgets_INCLUDE_DIRS}
  ${rqt_gui_cpp_INCLUDE_DIRS}
  )

qt5_wrap_cpp(rqt_example_cpp_MOCS ${rqt_example_cpp_HDRS})

qt5_wrap_ui(rqt_example_cpp_UIS_H ${rqt_example_cpp_UIS})

include_directories(${rqt_example_cpp_INCLUDE_DIRS})

add_library(${PROJECT_NAME} SHARED
  ${rqt_example_cpp_SRCS}
  ${rqt_example_cpp_MOCS}
  ${rqt_example_cpp_UIS_H})


target_link_libraries(
  ${PROJECT_NAME}
  ${rqt_gui_LIBRARIES}
  ${rqt_gui_cpp_LIBRARIES}
  Qt5::Widgets
  ${QT_LIBRARIES}
  ${QT_QTCORE_LIBRARY}
  ${QT_QTGUI_LIBRARY}
  ${rclcpp_LIBRARIES}
  ${std_msgs_LIBRARIES}
)

install(
  TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include)

install(PROGRAMS scripts/rqt_example_cpp
  DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY include/
  DESTINATION include
)

install(FILES plugin.xml
  DESTINATION share/${PROJECT_NAME}
)

pluginlib_export_plugin_description_file(rqt_gui "plugin.xml")
ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

ament_package()
