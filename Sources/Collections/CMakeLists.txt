#[[
This source file is part of the Swift Collections Open Source Project

Copyright (c) 2021 - 2024 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
#]]

add_library(Collections
    "Collections.swift")
target_link_libraries(Collections PRIVATE
  BitCollections
  DequeModule
  HeapModule
  OrderedCollections
  HashTreeCollections # maybe _RopeModule is missing?
  )

_install_target(Collections)
set_property(GLOBAL APPEND PROPERTY SWIFT_COLLECTIONS_EXPORTS Collections)

# Makes <exported> importable by any target listing <exporter> in its
# target_link_libraries.
function(swift_module_re_exports exporter exported)
  set(swift_module_directory "$<TARGET_PROPERTY:${exported},Swift_MODULE_DIRECTORY>")
  set(binary_directory "$<TARGET_PROPERTY:${exported},BINARY_DIR>")
  set_property(TARGET ${exporter}
    APPEND
    PROPERTY INTERFACE_INCLUDE_DIRECTORIES
  "$<IF:$<STREQUAL:${swift_module_directory},>,${binary_directory},${swift_module_directory}>")
endfunction()

# All the .exported targets from Package.swift
swift_module_re_exports(Collections BitCollections)
swift_module_re_exports(Collections DequeModule)
swift_module_re_exports(Collections HeapModule)
swift_module_re_exports(Collections OrderedCollections)
swift_module_re_exports(Collections HashTreeCollections) # maybe _RopeModule is missing?
