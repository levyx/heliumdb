cmake_minimum_required(VERSION 2.8.0)
project(heliumdb CXX)

# compile options
option(DEBUG "Enable debug build" OFF)
option(TESTS "Enable unittests" OFF)

find_package(Python COMPONENTS Development)

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
  set (CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE PATH "default install path" FORCE )
endif()

add_compile_options(-Wall)
if (DEBUG)
  message(STATUS "heliumdb DEBUG: ON")
  add_compile_options(-g3)
else ()
  message(STATUS "heliumdb DEBUG: OFF")
  add_compile_options(-g -O3)
endif (DEBUG)

# allow external projects
include(${CMAKE_ROOT}/Modules/ExternalProject.cmake)
include(${CMAKE_ROOT}/Modules/CheckIncludeFileCXX.cmake)

set(INCLUDE_PATHS
    ${PROJECT_SOURCE_DIR}/src/heliumdb/
    ${CMAKE_INSTALL_PREFIX}/include
)

SET(CMAKE_INSTALL_RPATH "/usr/lib")

configure_file(
  ${PROJECT_SOURCE_DIR}/${PROJECT_NAME}.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config-install.cmake
  @ONLY
  )

find_library (LIBHE
              NAMES he
              PATHS /usr /usr/local)
if(NOT LIBHE)
    message(FATAL_ERROR "failed to find libhe")
else()
    message(STATUS "found libhe: ${LIBHE}")
endif()

find_path(HE_H
          NAMES he.h
          PATHS /usr /usr/local
          )
if(NOT HE_H)
    message(FATAL_ERROR "failed to find he.h")
else()
    message(STATUS "found he.h: ${HE_H}")
    include_directories(${HE_H})
endif(NOT HE_H)

# add source
add_subdirectory(src)

if(TESTS)
  include(CTest)
  enable_testing()
  
  # add test dir if enabled
  add_subdirectory(test)
endif()
