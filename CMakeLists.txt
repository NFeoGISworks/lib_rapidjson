################################################################################
# Project:  CMake build scripts for RapidJSON library
# Purpose:  CMake build scripts
# Author:   Dmitry Baryshnikov, dmitry.baryshnikov@nexgis.com
# Author:   NikitaFeodonit, nfeodonit@yandex.com
################################################################################
# Copyright (C) 2016, NextGIS <info@nextgis.com>
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
################################################################################

cmake_minimum_required(VERSION 3.3.2 FATAL_ERROR)

project(librapidjson NONE)

# some init settings
set(CMAKE_COLOR_MAKEFILE ON)
# set path to additional CMake modules
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

include(GNUInstallDirs)
set(INSTALL_INC_DIR ${CMAKE_INSTALL_INCLUDEDIR} CACHE INTERNAL "Installation directory for headers" FORCE)
set(INSTALL_INC_FULL_DIR "${CMAKE_INSTALL_PREFIX}/${INSTALL_INC_DIR}")

include(util)
check_version(RAPIDJSON_MAJOR_VERSION RAPIDJSON_MINOR_VERSION RAPIDJSON_REV_VERSION)
set(VERSION ${RAPIDJSON_MAJOR_VERSION}.${RAPIDJSON_MINOR_VERSION}.${RAPIDJSON_REV_VERSION})
report_version(${PROJECT_NAME} ${VERSION})

configure_file(${CMAKE_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake IMMEDIATE @ONLY)

set(PUBLIC_HEADER_DIR "${CMAKE_SOURCE_DIR}/include")

set(PUBLIC_HDRS
    include/rapidjson/allocators.h
    include/rapidjson/document.h
    include/rapidjson/encodedstream.h
    include/rapidjson/encodings.h
    include/rapidjson/filereadstream.h
    include/rapidjson/filewritestream.h
    include/rapidjson/memorybuffer.h
    include/rapidjson/memorystream.h
    include/rapidjson/prettywriter.h
    include/rapidjson/rapidjson.h
    include/rapidjson/reader.h
    include/rapidjson/stringbuffer.h
    include/rapidjson/writer.h

    include/rapidjson/error/en.h
    include/rapidjson/error/error.h

    include/rapidjson/internal/biginteger.h
    include/rapidjson/internal/diyfp.h
    include/rapidjson/internal/dtoa.h
    include/rapidjson/internal/ieee754.h
    include/rapidjson/internal/itoa.h
    include/rapidjson/internal/meta.h
    include/rapidjson/internal/pow10.h
    include/rapidjson/internal/stack.h
    include/rapidjson/internal/strfunc.h
    include/rapidjson/internal/strtod.h

    include/rapidjson/msinttypes/inttypes.h
    include/rapidjson/msinttypes/stdint.h
)

set(LIB_NAME ${PROJECT_NAME})
add_library(${LIB_NAME} INTERFACE)
target_include_directories(${LIB_NAME} INTERFACE "${INSTALL_INC_FULL_DIR}")
target_sources(${LIB_NAME} INTERFACE ${PUBLIC_HDRS})

export(TARGETS ${LIB_NAME} FILE ${LIB_NAME}-exports.cmake)
add_custom_target(uninstall COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)

if(NOT SKIP_INSTALL_HEADERS AND NOT SKIP_INSTALL_ALL )
    install(DIRECTORY ${PUBLIC_HEADER_DIR}/ DESTINATION "${INSTALL_INC_DIR}")
endif()
