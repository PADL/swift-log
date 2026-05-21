#[[
This source file is part of the Swift Log open source project

Copyright (c) 2024 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
#]]

include(CMakePackageConfigHelpers)

set(SWIFT_LOG_EXPORTS_FILE
  ${CMAKE_CURRENT_BINARY_DIR}/SwiftLogExports.cmake)

configure_file(LoggingConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/LoggingConfig.cmake)

get_property(SWIFT_LOG_EXPORTS GLOBAL PROPERTY SWIFT_LOG_EXPORTS)
export(TARGETS ${SWIFT_LOG_EXPORTS}
  NAMESPACE Logging::
  FILE ${SWIFT_LOG_EXPORTS_FILE}
  EXPORT_LINK_INTERFACE_LIBRARIES)

install(EXPORT LoggingTargets
  FILE LoggingTargets.cmake
  NAMESPACE Logging::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Logging)

configure_package_config_file(
  LoggingConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/install/LoggingConfig.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Logging)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/install/LoggingConfig.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Logging)
