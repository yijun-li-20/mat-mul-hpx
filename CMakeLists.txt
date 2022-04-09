# Copyright (c) 2014-2016 Thomas Heller
#
# SPDX-License-Identifier: BSL-1.0
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

cmake_minimum_required(VERSION 3.18)

project(mat_mul CXX)

if(EXISTS "${HPX_DIR}")
  find_package(HPX REQUIRED)

  add_executable(mat_mul matmul.cpp)
  target_include_directories(mat_mul PRIVATE ${test_SOURCE_DIR})

  if("${SETUP_TYPE}" STREQUAL "TARGETS")

    target_link_libraries(mat_mul PRIVATE HPX::hpx HPX::wrap_main)
  else()
    message(FATAL_ERROR "Unknown SETUP_TYPE=\"${SETUP_TYPE}\"")
  endif()

  if(MSVC)
    # Only for the purposes of testing we output the executable and libraries to
    # the output directory of HPX
    set_target_properties(
      mat_mul PROPERTIES RUNTIME_OUTPUT_DIRECTORY
                                    ${HPX_OUTPUT_DIRECTORY}
    )

  endif()

  enable_testing()
  add_test(mat_mul_test mat_mul)
else()
  message(
    WARNING
      "Skipping build test because HPX_DIR=${HPX_DIR} does not exist. This \
      last test requires HPX to be installed.  Did you forget to run the \
      install rule?"
  )
endif()
