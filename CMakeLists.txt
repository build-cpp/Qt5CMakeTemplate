# This file was generated automatically by cmkr.

cmake_minimum_required(VERSION 3.15)

# Regenerate CMakeLists.txt automatically in the root project
set(CMKR_ROOT_PROJECT OFF)
if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
	set(CMKR_ROOT_PROJECT ON)

	# Bootstrap cmkr
	include("cmake/cmkr.cmake" OPTIONAL RESULT_VARIABLE CMKR_INCLUDE_RESULT)
	if(CMKR_INCLUDE_RESULT)
		cmkr()
	endif()

	# Enable folder support
	set_property(GLOBAL PROPERTY USE_FOLDERS ON)
endif()

# Create a configure-time dependency on cmake.toml to improve IDE support
if(CMKR_ROOT_PROJECT)
	configure_file(cmake.toml cmake.toml COPYONLY)
endif()

project(Qt5CMakeTemplate
	VERSION
		0.1.0
	DESCRIPTION
		"Simple template for Qt5 to get you started"
)

include("cmake/Qt5Helpers.cmake")

# Packages
find_package(Qt5 REQUIRED CONFIG
	COMPONENTS
		Widgets
)

# Target MyApplication
set(MyApplication_SOURCES
	"MyApplication/main.cpp"
	"MyApplication/MainWindow.cpp"
	"MyApplication/MainWindow.h"
	"MyApplication/MainWindow.ui"
	"MyApplication/resource.qrc"
	cmake.toml
)

add_executable(MyApplication ${MyApplication_SOURCES})

get_directory_property(CMKR_VS_STARTUP_PROJECT DIRECTORY ${PROJECT_SOURCE_DIR} DEFINITION VS_STARTUP_PROJECT)
if(NOT CMKR_VS_STARTUP_PROJECT)
	set_property(DIRECTORY ${PROJECT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT MyApplication)
endif()

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES ${MyApplication_SOURCES})

target_link_libraries(MyApplication PRIVATE
	Qt5::Widgets
)

# Copy Qt DLLs next to the application
target_windeployqt(MyApplication)

# Application without console
if(MSVC)
    target_link_options(MyApplication PRIVATE /SUBSYSTEM:WINDOWS)
endif()


install(
	TARGETS
		MyApplication
	DESTINATION
		bin
	COMPONENT
		MyApplication
)
