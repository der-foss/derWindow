cmake_minimum_required(VERSION 3.22)

project(derWindow)

set(CMAKE_C_STANDARD 99)

set(SOURCES
  src/window.c)

add_library(derWindow
  STATIC
  ${SOURCES})

if(WANDROID)
  set (CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate -u ANativeWindow_setFrameRateWithChangeStrategy")

  find_library(log-lib log)

  add_library (native_app_glue
    STATIC
    ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

  target_include_directories (native_app_glue
    PUBLIC
    ${ANDROID_NDK}/sources/android/native_app_glue)

  target_link_libraries(derWindow
    PRIVATE
    android
    ${log-lib}
    native_app_glue
    GLESv3)

  target_compile_definitions(derWindow
    PRIVATE
    __AANDROIDD__)
endif()

target_compile_options(derWindow
  PRIVATE
  -Wall
  -Werror
  -Wextra
  -pedantic
  -pedantic-errors)

target_include_directories(derWindow
  PUBLIC
  include/)

target_link_libraries(derWindow
  PRIVATE
  EGL)
