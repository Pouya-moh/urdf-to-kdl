project(KDL_Parser)
cmake_minimum_required(VERSION 2.8)

add_definitions(-fPIC)

file(GLOB KDL_Parser_SOURCES src/*.cpp)

include_directories(${PROJECT_SOURCE_DIR}/include)

find_package(orocos_kdl REQUIRED)
IF (orocos_kdl_FOUND)
	message(STATUS "orocos_kdl found. Version: ${orocos_kdl_VERSION}")
	include_directories(BEFORE SYSTEM ${orocos_kdl_INCLUDE_DIRS})
	add_definitions(${orocos_kdl_CFLAGS})
	link_directories(${orocos_kdl_LIBRARY_DIR})
ENDIF()

find_package(Boost REQUIRED COMPONENTS system)
IF (Boost_FOUND)
	message(STATUS "Boost found. Version: ${Boost_VERSION}")
	include_directories(BEFORE SYSTEM ${Boost_INCLUDE_DIRS})
	add_definitions(${Boost_CFLAGS})
	link_directories(${Boost_LIBRARY_DIR})
ENDIF()

find_package(urdfdom REQUIRED)
IF (urdfdom_FOUND)
	message(STATUS "urdfdom found. Version: ${Boost_VERSION}")
	include_directories(BEFORE SYSTEM ${urdfdom_INCLUDE_DIRS})
	add_definitions(${urdfdom_CFLAGS})
	link_directories(${urdfdom_LIBRARY_DIR})
ENDIF()


find_library(TinyXML NAMES tinyxml)

add_library(${PROJECT_NAME} STATIC ${KDL_Parser_SOURCES})
target_link_libraries(${PROJECT_NAME} ${orocos_kdl_LIBRARIES} ${TINYXML_LIBRARIES} ${TinyXML} ${Boost_LIBRARIES} ${urdfdom_LIBRARIES})

set(${PROJECT_NAME}_INCLUDE_DIRS 	${PROJECT_SOURCE_DIR}/include
									${Boost_INCLUDE_DIRS}
									${orocos_kdl_INCLUDE_DIRS}
									${urdfdom_INCLUDE_DIRS}
	CACHE INTERNAL "${PROJECT_NAME}: Include Directories" FORCE)

IF(BUILD_TEST)
	message(STATUS "Building with test executable")
	add_executable(test ${PROJECT_SOURCE_DIR}/test/check_kdl_parser.cpp)
	target_link_libraries(test ${PROJECT_NAME})
ENDIF()