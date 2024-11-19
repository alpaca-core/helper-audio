# Copyright (c) Alpaca Core
# SPDX-License-Identifier: MIT
#
CPMAddPackage(gh:iboB/doctest-util@0.1.3)
set_target_properties(doctest PROPERTIES FOLDER test)
set_target_properties(doctest-main PROPERTIES FOLDER test)

function(dl_audio url)
    get_filename_component(filename ${url} NAME)
    set(dlpath ${CMAKE_CURRENT_SOURCE_DIR}/${filename})
    if(NOT EXISTS ${dlpath})
        file(DOWNLOAD ${url} ${dlpath} SHOW_PROGRESS)
    endif()
endfunction()

dl_audio(https://huggingface.co/alpaca-core/ac-test-data-whisper/resolve/main/prentice-hall.wav)
dl_audio(https://huggingface.co/alpaca-core/ac-test-data-whisper/resolve/main/yes.wav)

add_library(ac-audio-test-data INTERFACE)
target_compile_definitions(ac-audio-test-data INTERFACE AC_AUDIO_TEST_FILES_DIR="${CMAKE_CURRENT_SOURCE_DIR}")

add_doctest_test(ac-audio
    SOURCES
        t-ac-audio.cpp
    LIBRARIES
        ac-dev::audio
        ac-audio-test-data
)
