project(svo_online_loopclosing)
cmake_minimum_required(VERSION 2.8.3)

SET(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS "-g -O0")


find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)
set(OpenCV_DIR /usr/share/OpenCV)
find_package(OpenCV REQUIRED)

if(${OpenCV_VERSION} STREQUAL "3.2.0")
  message(STATUS "OpenCV version ${OpenCV_VERSION}")
else()
  message(WARNING "OpenCV version ${OpenCV_VERSION}, OpenCV version must be 3.2.0")
endif()

include(SvoSetup)

set(HEADERS
  include/svo/online_loopclosing/bow.h
  include/svo/online_loopclosing/geometric_verification.h
  include/svo/online_loopclosing/loop_closing.h
  include/svo/online_loopclosing/read_file.h
  include/svo/online_loopclosing/keyframe.h
  include/svo/online_loopclosing/loop_closing_types.h
  include/svo/online_loopclosing/map_alignment.h
  )

set(SOURCES
  src/bow.cpp
  src/geometric_verification.cpp
  src/loop_closing.cpp
  src/read_file.cpp
  src/map_alignment.cpp
  )

#add_executable(svo_online_loopclosing ${SOURCES} ${HEADERS})
cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})

cs_add_library(bow src/bow.cpp include/svo/online_loopclosing/bow.h)
target_link_libraries(bow ${OpenCV_LIBRARIES})

cs_add_executable(createvoc test/createvoc.cpp)
target_link_libraries(createvoc bow)

cs_install()
cs_export()
