cmake_minimum_required(VERSION 3.5)
project(dummy_lanelet2)

add_compile_options(-Wall -Wextra -Wpedantic)

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

find_package(lanelet2 REQUIRED)

include_directories(include
        ${lanelet2_INCLUDE_DIRS})

set(DUMMY_LANELET2_LIB_SRC
        src/dummy_lanelet2.cpp)

set(DUMMY_LANELET2_LIB_HEADERS
        include/dummy_lanelet2/dummy_lanelet2.hpp)


ament_auto_add_library(${PROJECT_NAME} SHARED
        ${DUMMY_LANELET2_LIB_HEADERS}
        ${DUMMY_LANELET2_LIB_SRC})
target_link_libraries(${PROJECT_NAME}
        ${lanelet2_LIBRARIES})

#target_compile_options(${PROJECT_NAME} PRIVATE
#  -Wall
#  -Werror
#  -Wextra
#  #-Wshadow             # causes issues with ROS 2 headers
#  #-Wnon-virtual-dtor   # causes issues with ROS 2 headers
#  -pedantic
#  -Wcast-align
#  -Wunused
#  -Wconversion
#  -Wsign-conversion
#  -Wdouble-promotion
#  #-Wnull-dereference    # gcc6
#  #-Wduplicated-branches # gcc7
#  #-Wduplicated-cond     # gcc6
#  #-Wrestrict            # gcc7
#  -fvisibility=hidden)
# C++-only options

rclcpp_components_register_node(${PROJECT_NAME}
        PLUGIN "dummy_lanelet2::DummyLanelet2"
        EXECUTABLE ${PROJECT_NAME}_exe)

if(BUILD_TESTING)
    find_package(ament_lint_auto REQUIRED)
    ament_lint_auto_find_test_dependencies()
endif()

install(DIRECTORY
        launch
        DESTINATION share/${PROJECT_NAME}/
)
install(DIRECTORY
        config
        DESTINATION share/${PROJECT_NAME}
)

ament_export_include_directories(include)
ament_export_libraries({${PROJECT_NAME}_node})

ament_auto_package()