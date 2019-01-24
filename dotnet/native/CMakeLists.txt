# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT license.

cmake_minimum_required(VERSION 3.10)

project(SEALDLL VERSION 3.1.0 LANGUAGES CXX)

# Library will be in ../../lib
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/../../lib)

# Create library but add no source files yet
add_library(sealdll SHARED "")

# Import Microsoft SEAL
find_package(SEAL 3.1.0 EXACT REQUIRED)

# Check that Microsoft SEAL is build as position-independent code
if (SEAL_LIB_BUILD_TYPE STREQUAL "Static")
    message(FATAL_ERROR "Microsoft SEAL is not built as position-independent code; please set SEAL_LIB_BUILD_TYPE to either \"Static_PIC\" or \"Shared\" and rebuild.")
endif()

# Link Microsoft SEAL
target_link_libraries(sealdll SEAL::seal)

# Add source files to library
add_subdirectory(seal)

# Install only the shared library. This library is only meant to be
# used by the SEALNet .NET library.
install(TARGETS sealdll LIBRARY DESTINATION lib)