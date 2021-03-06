cmake_minimum_required(VERSION 2.8)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
project(linemod_ros)
find_package(PCL 1.7 REQUIRED COMPONENTS common io recognition segmentation visualization)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})
if (NOT PCL_FOUND)
    MESSAGE(FATAL_ERROR "PCL not found.\n")
endif (NOT PCL_FOUND)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)

rosbuild_add_executable(train_virtual src/train_virtual.cpp
                                        Linemod/VirtualModeller.cpp
                                        Linemod/LinemodWrap.cpp)
target_link_libraries(train_virtual ${PCL_COMMON_LIBRARIES}
                                     ${PCL_IO_LIBRARIES}
                                     ${PCL_RECOGNITION_LIBRARIES}
                                     ${PCL_SEGMENTATION_LIBRARIES}
                                     ${PCL_VISUALIZATION_LIBRARIES}
                                     libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkHybrid.so)

rosbuild_add_executable(train_online src/train_online.cpp
                                     Linemod/OnlineGrabber.cpp
                                     Linemod/LinemodWrap.cpp)
target_link_libraries(train_online ${PCL_COMMON_LIBRARIES}
                                   ${PCL_IO_LIBRARIES}
                                   ${PCL_RECOGNITION_LIBRARIES}
                                   ${PCL_SEGMENTATION_LIBRARIES}
                                   ${PCL_VISUALIZATION_LIBRARIES}
                                   libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkHybrid.so)

rosbuild_add_executable(detect src/detect.cpp
                               Linemod/LinemodWrap.cpp)
target_link_libraries(detect ${PCL_COMMON_LIBRARIES}
                             ${PCL_IO_LIBRARIES}
                             ${PCL_RECOGNITION_LIBRARIES}
                             ${PCL_SEGMENTATION_LIBRARIES}
                             ${PCL_VISUALIZATION_LIBRARIES}
                             libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkHybrid.so)
                                 
rosbuild_add_executable(detect_test_offline src/detect_test_offline.cpp
                                            Linemod/LinemodWrap.cpp)
target_link_libraries(detect_test_offline ${PCL_COMMON_LIBRARIES}
                                         ${PCL_IO_LIBRARIES}
                                         ${PCL_RECOGNITION_LIBRARIES}
                                         ${PCL_SEGMENTATION_LIBRARIES}
                                         ${PCL_VISUALIZATION_LIBRARIES}
                                         libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkHybrid.so)
                                         
                                         

