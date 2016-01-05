cmake_minimum_required (VERSION 2.8)

# define a macro that helps defining an option

# project name
set(PROJECT_NAME "Project")
project (${PROJECT_NAME})

# setup version numbers
set(VERSION_MAJOR 0)
set(VERSION_MINOR 0)
set(VERSION_PATCH 0)

include(cmake/macro.cmake)
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules;${CMAKE_SOURCE_DIR}")


# add the  header path
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${CMAKE_CURRENT_SOURCE_DIR}/src)

#add an option for building the examples
set_option(UTILS_BUILD_EXAMPLES TRUE BOOL "TRUE to build the examples, FALSE to ignore them")

# add an option for building the API documentation
set_option(UTILS_BUILD_DOC TRUE BOOL "TRUE to generate the API documentation, FALSE to ignore it")

set(LIBS "")

set(DEFINES "")
set(WARNING "-Wall")
set(FLAGS "-g -std=c++0x")
set(CMAKE_CXX_FLAGS "${DEFINES} ${WARNING} ${FLAGS}")

set(OUTPUT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lib")

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)
set(LIB_INSTALL_DIR ${CMAKE_CURRENT_SOURCE_DIR}/lib)

# Second, for multi-config builds (e.g. msvc)
foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG )
    set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${OUTPUT_DIR} )
    set( CMAKE_LIBRARY_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${OUTPUT_DIR} )
    set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${OUTPUT_DIR} )
endforeach( OUTPUTCONFIG CMAKE_CONFIGURATION_TYPES )

# add the subdirectories
add_subdirectory(src/${PROJECT_NAME})
if(UTILS_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
#build doc
if(UTILS_BUILD_DOC)
    add_subdirectory(doc)
endif()

