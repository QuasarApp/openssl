#
# Copyright (C) 2018-2020 QuasarApp.
# Distributed under the lgplv3 software license, see the accompanying
# Everyone is permitted to copy and distribute verbatim copies
# of this license document, but changing it is not allowed.
#

cmake_minimum_required(VERSION 3.1)

project(openssl LANGUAGES C)


set (BUILD_OUT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/build)

if(UNIX)
    message("build cmake for unix system")

    ADD_CUSTOM_TARGET( prepare
            COMMAND ./config --prefix=${BUILD_OUT_DIR} --openssldir=${BUILD_OUT_DIR} shared
            WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../
    )

elseif(WIN32)
    message("build cmake for win32 system")

elseif(ANDROID)
    message("build cmake for android system")

else ()
    message("not supported platform build !!!")
endif()

ADD_CUSTOM_TARGET( ${PROJECT_NAME}Make
        COMMAND ${CMAKE_MAKE_PROGRAM} -j4
        COMMENT "${CMAKE_MAKE_PROGRAM} -j4"
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../
        DEPENDS prepare
)

ADD_CUSTOM_TARGET( ${PROJECT_NAME} ALL
        COMMAND ${CMAKE_MAKE_PROGRAM} install
        COMMENT "${CMAKE_MAKE_PROGRAM} -j4"
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../
        DEPENDS ${PROJECT_NAME}Make
)

set (IMPORTED_LOCATION ${BUILD_OUT_DIR}/lib PARENT_SCOPE)
set(PUBLIC_HEADER ${BUILD_OUT_DIR}/include PARENT_SCOPE)

add_library(ssl SHARED IMPORTED)

add_library(crypto SHARED IMPORTED)




