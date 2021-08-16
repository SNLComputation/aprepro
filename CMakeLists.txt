cmake_minimum_required(VERSION 3.12.0...3.21.0)

project(aprepro VERSION 4.2.0 LANGUAGES C CXX)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

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
  PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
  "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")
install(
  TARGETS aprepro
  EXPORT aprepro-targets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/config.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/aprepro-config.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/aprepro
)
write_basic_package_version_file(
  "${PROJECT_BINARY_DIR}/aprepro-config-version.cmake"
  COMPATIBILITY AnyNewerVersion)

install(FILES
  "${PROJECT_BINARY_DIR}/aprepro-config.cmake"
  "${PROJECT_BINARY_DIR}/aprepro-config-version.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/aprepro)

install(
  EXPORT aprepro-targets
  NAMESPACE aprepro::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/aprepro)
