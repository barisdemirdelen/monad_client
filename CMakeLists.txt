

project(monad_client)
set(APP monad_client)

cmake_minimum_required(VERSION 2.8)

set(OF_ROOT "~/lib/of" CACHE PATH "The root directory of ofnode/of project.")
#set(ADDON_DEPENDENCIES ofxTonic ofxUI ofxMSAPerlin)

include(${OF_ROOT}/openFrameworks.cmake)

ofxaddon(ofxTonic)
ofxaddon(ofxUI)
ofxaddon(ofxMSAPerlin)
ofxaddon(ofxXmlSettings)
ofxaddon(ofxNetwork)

option(COTIRE "Use cotire" ON)

set(SOURCES
        ofxTonic/src/Tonic/BLEPOscillator.h
        ofxTonic/src/Tonic/BLEPOscillator.cpp
        src/Disc.cpp
        src/Disc.h
        src/Groove.cpp
        src/Groove.h
        src/main.cpp
        src/ofApp.cpp
        src/ofApp.h
        src/Player.cpp
        src/Player.h
        src/Sound.cpp
        src/Sound.h)

add_executable(
        ${APP}
        ${SOURCES}
        ${OFXADDONS_SOURCES}
)

target_link_libraries(
        ${APP}
        ${OPENFRAMEWORKS_LIBRARIES}
)

set_target_properties(
        ${APP} PROPERTIES
        MACOSX_BUNDLE_INFO_PLIST
        "${CMAKE_CURRENT_SOURCE_DIR}/Info.plist"
)

if(CMAKE_BUILD_TYPE MATCHES Debug)
    set_target_properties( ${APP} PROPERTIES OUTPUT_NAME "${APP}-Debug")
endif()

if (CMAKE_CROSSCOMPILING)
    set_target_properties( ${APP} PROPERTIES OUTPUT_NAME
            "${APP}-${OF_PLATFORM}-${CMAKE_BUILD_TYPE}")
endif()

if (COTIRE)
    cotire(${APP})
endif()