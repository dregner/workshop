cmake_minimum_required(VERSION 2.8.3)
project(workshop_gazebo)

find_package(catkin REQUIRED)

catkin_package()


install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

install(DIRECTORY worlds
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

find_package(catkin REQUIRED COMPONENTS gazebo_msgs roscpp rospy sensor_msgs cv_bridge
        image_transport
        std_msgs)

catkin_package(
        CATKIN_DEPENDS
        gazebo_msgs
        roscpp
        sensor_msgs
)
include_directories(include ${catkin_INCLUDE_DIRS})

if (${gazebo_VERSION_MAJOR} LESS 6)
    # These two
    include(FindBoost)
    find_package(Boost ${MIN_BOOST_VERSION} REQUIRED system filesystem regex)
    target_link_libraries(velocidade ${GAZEBO_LIBRARIES} ${Boost_LIBRARIES})
else ()
    target_link_libraries(velocidade ${GAZEBO_LIBRARIES})
endif ()

foreach (dir launch models resource worlds)
    install(DIRECTORY ${dir}/
            DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach (dir)