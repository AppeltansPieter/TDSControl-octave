cmake_minimum_required(VERSION 3.16.0)

file(READ "${CMAKE_SOURCE_DIR}/TDSControl/VERSION" VERSION_CONTENTS) 
string(REGEX MATCH "VERSION = \"([0-9]+\.[0-9]+\.[0-9])\"" _ ${VERSION_CONTENTS})
set(PROJECT_VERSION "${CMAKE_MATCH_1}")

add_subdirectory("${CMAKE_SOURCE_DIR}/TDSControl")

project(tds-control-octave VERSION ${PROJECT_VERSION} LANGUAGES C CXX)
find_program(MKOCTFILE_EXECUTABLE mkoctfile)

if(NOT MKOCTFILE_EXECUTABLE)
    message(FATAL_ERROR "mkoctfile not found")
endif()

get_target_property(TDSCONTROL_INCLUDES
    tdscontrol
    INTERFACE_INCLUDE_DIRECTORIES
)

if(TDSCONTROL_INCLUDES)
    list(TRANSFORM TDSCONTROL_INCLUDES PREPEND "-I")
endif()

add_custom_command(
    OUTPUT tds_create.oct
    COMMAND ${MKOCTFILE_EXECUTABLE}
            -std=c++17
            ${TDSCONTROL_INCLUDES}
            ${CMAKE_CURRENT_SOURCE_DIR}/src/tds_create.cpp
            $<TARGET_FILE:tdscontrol>
            -o tds_create.oct
    DEPENDS tdscontrol src/tds_create.cpp
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

add_custom_command(
    OUTPUT tds_roots.oct
    COMMAND ${MKOCTFILE_EXECUTABLE}
            -std=c++17
            ${TDSCONTROL_INCLUDES}
            ${CMAKE_CURRENT_SOURCE_DIR}/octave/tds_roots.cpp
            $<TARGET_FILE:tdscontrol>
            -o tds_roots.oct
    DEPENDS tdscontrol ${CMAKE_CURRENT_SOURCE_DIR}/src/tds_roots.cpp
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

add_custom_target(octave_bindings ALL
    DEPENDS tds_roots.oct tds_create.oct
)
