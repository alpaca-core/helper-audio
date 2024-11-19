# Copyright (c) Alpaca Core
# SPDX-License-Identifier: MIT
#
include(icm_add_lib)

icm_add_lib(ac-audio AC_AUDIO)
add_library(ac-dev::audio ALIAS ac-audio)

if(NOT TARGET dr_libs)
    CPMAddPackage(
        NAME dr_libs
        VERSION 1.0
        GIT_REPOSITORY https://github.com/mackron/dr_libs.git
        GIT_TAG da35f9d
    )
    add_library(dr_libs INTERFACE)
    target_include_directories(dr_libs INTERFACE ${dr_libs_SOURCE_DIR})
endif()

CPMAddPackage(gh:iboB/splat@1.3.3)
CPMAddPackage(gh:iboB/itlib@1.11.4)

target_link_libraries(ac-audio
    PUBLIC
        splat::splat
    PRIVATE
        dr_libs
        itlib::itlib
)

target_sources(ac-audio
    INTERFACE FILE_SET HEADERS FILES
        ac-audio.hpp
    PRIVATE
        ac-audio.cpp
)
