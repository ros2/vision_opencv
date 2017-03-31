cmake_minimum_required(VERSION 3.5)
project(image_geometry)

find_package(ament_cmake REQUIRED)

if(NOT WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wall -Wextra")
endif()

find_package(OpenCV REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(Boost REQUIRED)

# catkin_package(CATKIN_DEPENDS sensor_msgs
#                DEPENDS OpenCV
#                INCLUDE_DIRS include
#                LIBRARIES ${PROJECT_NAME}
# )

# catkin_python_setup()

#include_directories(${catkin_INCLUDE_DIRS}
#                           ${OpenCV_INCLUDE_DIRS}
#)
#include_directories(include)

include_directories(include
                    ${OpenCV_INCLUDE_DIRS}
)

# add a library
add_library(${PROJECT_NAME} src/pinhole_camera_model.cpp src/stereo_camera_model.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})
ament_target_dependencies(${PROJECT_NAME} "sensor_msgs" "OpenCV")
#add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION include/${PROJECT_NAME}
)

install(TARGETS ${PROJECT_NAME}
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
)

# add tests
if(BUILD_TESTING)
#  add_subdirectory(test)
endif()

ament_export_libraries(${PROJECT_NAME})
ament_export_dependencies(OpenCV)
ament_package()
