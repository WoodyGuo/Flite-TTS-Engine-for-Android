set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

cmake_minimum_required(VERSION 3.4.1)


set (APP_ROOT_DIR "../../..")
set (IMPL_ROOT_DIR "src/main/jni")

set( IMPL_SRC ${IMPL_ROOT_DIR}/edu_cmu_cs_speech_tts_flite_engine.cc
              ${IMPL_ROOT_DIR}/edu_cmu_cs_speech_tts_flite_service.cc
              ${IMPL_ROOT_DIR}/edu_cmu_cs_speech_tts_flite_voices.cc
              ${IMPL_ROOT_DIR}/edu_cmu_cs_speech_tts_string.cc
              )

set (FLITE_HOME "${APP_ROOT_DIR}/deps/flite")

file(GLOB FLITE_AUDIO_SRC "${FLITE_HOME}/src/audio/*.c")
file(GLOB FLITE_CG_SRC "${FLITE_HOME}/src/cg/*.c")
file(GLOB FLITE_HRG_SRC "${FLITE_HOME}/src/hrg/*.c")
file(GLOB FLITE_LEXICON_SRC "${FLITE_HOME}/src/lexicon/*.c")
file(GLOB FLITE_REGEX_SRC "${FLITE_HOME}/src/regex/*.c")
file(GLOB FLITE_SPEECH_SRC "${FLITE_HOME}/src/speech/*.c")
file(GLOB FLITE_STATS_SRC "${FLITE_HOME}/src/stats/*.c")
file(GLOB FLITE_SYNTH_SRC "${FLITE_HOME}/src/synth/*.c")
file(GLOB FLITE_UTILS_SRC "${FLITE_HOME}/src/utils/*.c")
file(GLOB FLITE_WAVESYNTH_SRC "${FLITE_HOME}/src/wavesynth/*.c")

list(FILTER FLITE_UTILS_SRC EXCLUDE REGEX ".*win32.c$")
list(FILTER FLITE_UTILS_SRC EXCLUDE REGEX ".*posix.c$")
list(FILTER FLITE_UTILS_SRC EXCLUDE REGEX ".*palmos.c$")
list(FILTER FLITE_UTILS_SRC EXCLUDE REGEX ".*wince.c$")

list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*wince.c$")
list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*win.c$")
list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*sun.c$")
list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*alsa.c$")
list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*pulseaudio.c$")
list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*oss.c$")
list(FILTER FLITE_AUDIO_SRC EXCLUDE REGEX ".*palmos.c$")

file(GLOB FLITE_LANG_USENGLISH_SRC "${FLITE_HOME}/lang/usenglish/*.c")
file(GLOB FLITE_LANG_CMULEX_SRC "${FLITE_HOME}/lang/cmulex/*.c")
file(GLOB FLITE_LANG_CMU_US_SLT_SRC "${FLITE_HOME}/lang/cmu_us_slt/*.c")
file(GLOB FLITE_LANG_CMU_INDIC_LANG_SRC "${FLITE_HOME}/lang/cmu_indic_lang/*.c")
file(GLOB FLITE_LANG_CMU_INDIC_LEX_SRC "${FLITE_HOME}/lang/cmu_indic_lex/*.c")
file(GLOB FLITE_LANG_CMU_GRAPHEME_LANG_SRC "${FLITE_HOME}/lang/cmu_grapheme_lang/*.c")
file(GLOB FLITE_LANG_CMU_GRAPHEME_LEX_SRC "${FLITE_HOME}/lang/cmu_grapheme_lex/*.c")

list(FILTER FLITE_LANG_CMULEX_SRC EXCLUDE REGEX ".*raw.c$")
list(FILTER FLITE_LANG_CMULEX_SRC EXCLUDE REGEX ".*table.c$")
list(FILTER FLITE_LANG_CMULEX_SRC EXCLUDE REGEX ".*bytes.c$")

set( FLITE_SRC

     ${FLITE_AUDIO_SRC}
     ${FLITE_CG_SRC}
     ${FLITE_HRG_SRC}
     ${FLITE_LEXICON_SRC}
     ${FLITE_REGEX_SRC}
     ${FLITE_SPEECH_SRC}
     ${FLITE_STATS_SRC}
     ${FLITE_SYNTH_SRC}
     ${FLITE_UTILS_SRC}
     ${FLITE_WAVESYNTH_SRC}

     ${FLITE_LANG_USENGLISH_SRC}
     ${FLITE_LANG_CMULEX_SRC}

     ${FLITE_LANG_CMU_INDIC_LEX}
     ${FLITE_LANG_CMU_INDIC_LANG_SRC}
     ${FLITE_LANG_CMU_GRAPHEME_LEX_SRC}
     ${FLITE_LANG_CMU_GRAPHEME_LANG_SRC}

     ${FLITE_LANG_CMU_US_SLT_SRC} )

add_library( native-lib

             SHARED

             ${IMPL_SRC}
             ${FLITE_SRC}
             )

include_directories( ${IMPL_ROOT_DIR}
                     ${FLITE_HOME}/include
                     ${FLITE_HOME}/lang/usenglish
                     ${FLITE_HOME}/lang/cmulex
                     ${FLITE_HOME}/lang/cmu_us_slt )

add_definitions( -DCST_AUDIO_NONE -fpic -DANDROID )

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

target_link_libraries( native-lib
                       ${log-lib} )
