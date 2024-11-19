# Copyright (c) Alpaca Core
# SPDX-License-Identifier: MIT
#
cmake_minimum_required(VERSION 3.24 FATAL_ERROR)

project(ac-audio
    VERSION 1.0.0
    DESCRIPTION "Alpaca Core audio helper library"
    LANGUAGES CXX
)

# cpm
include(./get_cpm.cmake)

#######################################
# cmake lib
CPMAddPackage(gh:iboB/icm@1.5.4)

list(APPEND CMAKE_MODULE_PATH
    "${icm_SOURCE_DIR}"
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake"
)

set(ICM_DEV_C_STANDARD 11)
set(ICM_DEV_CXX_STANDARD 20)

include(icm_dev_mode)

#######################################
# options

option(AC_AUDIO_BUILD_TESTS "${PROJECT_NAME}: build tests" ${ICM_DEV_MODE})
option(AC_AUDIO_BUILD_EXAMPLES "${PROJECT_NAME}: build examples" ${ICM_DEV_MODE})
mark_as_advanced(AC_AUDIO_BUILD_TESTS AC_AUDIO_BUILD_EXAMPLES)

#######################################
# code

add_subdirectory(code)

if(AC_AUDIO_BUILD_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()
