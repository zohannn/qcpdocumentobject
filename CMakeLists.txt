cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "-g -Wall -std=c++11")
set(PROJECT_NAME qcpdocumentobject)
project (${PROJECT_NAME})

## Defult build type: Debug ##
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif()
## Debug definition: ##
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  add_definitions("-DDEBUG")
endif()
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O0")

find_package(Qt4 COMPONENTS 
					QtCore 
					QtGui 
					QApplication
)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
include(${QT_USE_FILE})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

# -- QCustomPlot
add_definitions(-DQCUSTOMPLOT_USE_LIBRARY)
set(QCUSTOMPLOT_HOME /home/$ENV{USER}/qcustomplot)
include_directories(
    ${QCUSTOMPLOT_HOME}/include
)
link_directories(
    ${QCUSTOMPLOT_HOME}/build
)

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
	add_library(${PROJECT_NAME}d SHARED src/qcpdocumentobject.cpp)
	target_link_libraries(${PROJECT_NAME}d qcustomplotd)
	install(TARGETS ${PROJECT_NAME}d DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/build)
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
	add_library(${PROJECT_NAME} SHARED src/qcpdocumentobject.cpp)
	target_link_libraries(${PROJECT_NAME} qcustomplot)
	install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/build)
else()
	MESSAGE(FATAL_ERROR "Please specify -DCMAKE_BUILD_TYPE=Debug|Release")
endif()


