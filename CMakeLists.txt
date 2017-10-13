##
## CMakeLists.txt for AIKO
## STM32F10x firmware
##

##
## maintainer Johann Lee <me@qinka.pro>
## Copyright (C) 2017 Johann Lee <me@qinka.pro>
## GPLv3
##

## require for CMake
cmake_minimum_required(VERSION 3.3)

## build system set up
set (CMAKE_SYSTEM_NAME      Generic)
set (CMAKE_SYSTEM_PROCESSOR arm)
set (CMAKE_CROSSCOMPILING   TRUE)

## project
project(aiko-stm3ff10x-firmware)

## languagse
enable_language(C ASM)

## config the aiko
set(AIKO_PATH_CONFIG "config")
include("${AIKO_PATH_CONFIG}/config.cmake")

## for platform
if (CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")
  if (NOT (DEFINED "EXE_IGNORE"))
    set (EXE_POSTFIX ".exe") 
  endif (NOT (DEFINED "EXE_IGNORE"))
endif (CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")


## add include path
include_directories (include/firmware)

## add source files
#if(DEFINED AIKO_DEBUG_WATCH_SRC_FILE)
  variable_watch(FIRMWARE_SRC_FILE)
#endif (DEFINED AIKO_DEBUG_WATCH_SRC_FILE)
## firmware
aux_source_directory(source FIRMWARE_SRC_FILE)
## to build library
add_library(stm32f10x STATIC ${FIRMWARE_SRC_FILE})
## install files
## target
install(TARGETS stm32f10x DESTINATION lib)
## include files
install(DIRECTORY include/firmware DESTINATION include)
