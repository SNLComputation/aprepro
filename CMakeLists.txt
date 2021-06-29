cmake_minimum_required(VERSION 3.12.0...3.21.0)

project(aprepro VERSION 4.2.0 LANGUAGES C CXX)

if(WIN32 AND NOT CYGWIN)
  set(INSTALL_CMAKE_DIR CMake)
else()
  set(INSTALL_CMAKE_DIR lib/cmake/${PROJECT_NAME})
endif()

set(APREPRO_HEADERS
  aprepro.h
  )

set(APREPRO_SOURCES
  apr_aprepro.cc
  apr_util.cc
  apr_stats.cc
  apr_init.cc
  apr_builtin.cc
  apr_units.cc
  apr_tokenize.cc
  apr_scanner.cc
  apr_getline_int.c
  apr_parser.cc
  apr_aprepro.cc
  apr_array.cc
  )

add_library(aprepro ${APREPRO_SOURCES})
target_compile_features(aprepro PUBLIC cxx_std_11)
set_target_properties(aprepro PROPERTIES
  PUBLIC_HEADER "${APREPRO_HEADERS}")
target_include_directories(aprepro
  PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")
target_include_directories(aprepro
  INTERFACE $<INSTALL_INTERFACE:include>)
install(TARGETS aprepro EXPORT aprepro-targets)

configure_file(aprepro-config.cmake.in
  "${PROJECT_BINARY_DIR}/aprepro-config.cmake" @ONLY)
configure_file(aprepro-config-version.cmake.in
  "${PROJECT_BINARY_DIR}/aprepro-config-version.cmake" @ONLY)

install(FILES
  "${PROJECT_BINARY_DIR}/aprepro-config.cmake"
  "${PROJECT_BINARY_DIR}/aprepro-config-version.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)

install(EXPORT aprepro-targets
  DESTINATION "${INSTALL_CMAKE_DIR}")
