#[[
This source file is part of the Swift Log open source project

Copyright (c) 2024 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
#]]

file(GLOB_RECURSE LOGGING_SOURCES CONFIGURE_DEPENDS
  *.swift)

add_library(Logging ${LOGGING_SOURCES})

# Logging uses `package` access-level declarations; SPM supplies -package-name
# implicitly.
target_compile_options(Logging PRIVATE
  "$<$<COMPILE_LANGUAGE:Swift>:SHELL:-package-name swift-log>")

# Upcoming-features applied to every regular target by Package.swift.
foreach(_feature ExistentialAny MemberImportVisibility InternalImportsByDefault
                  NonisolatedNonsendingByDefault)
  target_compile_options(Logging PRIVATE
    "$<$<COMPILE_LANGUAGE:Swift>:SHELL:-enable-upcoming-feature ${_feature}>")
endforeach()
target_compile_options(Logging PRIVATE
  "$<$<COMPILE_LANGUAGE:Swift>:SHELL:-Xfrontend -require-explicit-sendable>")

get_swift_host_os(_swift_os)
set_target_properties(Logging PROPERTIES
  SOVERSION 0
  INTERFACE_INCLUDE_DIRECTORIES
    "$<BUILD_INTERFACE:${CMAKE_Swift_MODULE_DIRECTORY}>;$<INSTALL_INTERFACE:${CMAKE_INSTALL_LIBDIR}/swift/${_swift_os}>")

_install_target(Logging)
set_property(GLOBAL APPEND PROPERTY SWIFT_LOG_EXPORTS Logging)
