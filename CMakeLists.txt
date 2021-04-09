#
# Copyright (C) 2021-2021 QuasarApp.
# Distributed under the lgplv3 software license, see the accompanying
# Everyone is permitted to copy and distribute verbatim copies
# of this license document, but changing it is not allowed.
#

cmake_minimum_required(VERSION 3.12)

project(OpenSSL)
if(TARGET ${PROJECT_NAME})
  message("The ${PROJECT_NAME} arledy included in main Project")
  return()
endif()

include(CMake/QuasarAppCITargets.cmake)

initAll()

if (DEFINED TARGET_PLATFORM_TOOLCHAIN)
    if (${TARGET_PLATFORM_TOOLCHAIN} STREQUAL "wasm32")
        return()
    endif()
endif()

set(SSL_PREFIX_DIR ${CMAKE_CURRENT_SOURCE_DIR}/sslRoot)

ADD_CUSTOM_TARGET(
    ${PROJECT_NAME}_Configure
    COMMAND ./config --prefix=${SSL_PREFIX_DIR} --openssldir=${SSL_PREFIX_DIR}
    COMMENT "./Configure --prefix=${SSL_PREFIX_DIR} --openssldir=${SSL_PREFIX_DIR}"
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)


ADD_CUSTOM_TARGET(
    ${PROJECT_NAME}_Make
    COMMAND ${CMAKE_MAKE_PROGRAM} -${CMAKE_BUILD_PARALLEL_LEVEL}
    COMMENT "${CMAKE_MAKE_PROGRAM} -${CMAKE_BUILD_PARALLEL_LEVEL}"
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

ADD_CUSTOM_TARGET(
    ${PROJECT_NAME}_Install
    COMMAND ${CMAKE_MAKE_PROGRAM} install
    COMMENT "${CMAKE_MAKE_PROGRAM} install"
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

ADD_CUSTOM_TARGET(
    ${PROJECT_NAME} ALL
    COMMENT "Build OpenSSL"
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_Install)
add_dependencies(${PROJECT_NAME}_Install ${PROJECT_NAME}_Make)
add_dependencies(${PROJECT_NAME}_Make ${PROJECT_NAME}_Configure)

set(OPENSLL_ROOT_DIR ${SSL_PREFIX_DIR} PARENT_SCOPE)
