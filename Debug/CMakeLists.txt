cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "-g -Wall -std=c++11")
set(PROJECT_NAME qcpdocumentobjectd)
project (${PROJECT_NAME})

set(CMAKE_BUILD_TYPE Debug)
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
    ${QCUSTOMPLOT_HOME}/${CMAKE_BUILD_TYPE}/include
)
link_directories(
    ${QCUSTOMPLOT_HOME}/${CMAKE_BUILD_TYPE}/lib
)

add_library(${PROJECT_NAME} SHARED src/qcpdocumentobject.cpp)
target_link_libraries(${PROJECT_NAME} qcustomplotd)
install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/lib)