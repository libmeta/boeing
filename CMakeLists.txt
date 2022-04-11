cmake_minimum_required(VERSION 3.5)

project(boeing LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wall -O2")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake")

include(IncludeDirectoryRecursive)

include_directory_recursive(${CMAKE_CURRENT_SOURCE_DIR})
foreach(DIR_NAME  util connection message transport estimator pacing)
    file(GLOB_RECURSE ${DIR_NAME}_CC ${CMAKE_CURRENT_SOURCE_DIR}/${DIR_NAME}/*.cc)
    set(LINK_SOURCES_CC ${LINK_SOURCES_CC} ${${DIR_NAME}_CC})
endforeach(DIR_NAME)

set(LINK_SOURCES_CC
    ${LINK_SOURCES_CC}
    ${CMAKE_CURRENT_SOURCE_DIR}/Loop.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/Session.cc
    )

add_executable(boeing_serv  Main.cc ${LINK_SOURCES_CC})
target_compile_definitions( boeing_serv PRIVATE CATCH_CONFIG_MAIN)

add_executable(boeing_client  ${CMAKE_CURRENT_SOURCE_DIR}/client/UdpClient.cc ${LINK_SOURCES_CC})
