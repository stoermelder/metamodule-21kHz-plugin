cmake_minimum_required(VERSION 3.22)

# Set the path to the Plugin SDK.
if(NOT "${METAMODULE_SDK_DIR}" STREQUAL "")
	message("METAMODULE_SDK_DIR set by CMake variable ${METAMODULE_SDK_DIR}")
elseif (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
	message("METAMODULE_SDK_DIR set by environment variable ${METAMODULE_SDK_DIR}")
else()
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
	message("METAMODULE_SDK_DIR set to default: ${METAMODULE_SDK_DIR}")
endif()

include(${METAMODULE_SDK_DIR}/plugin.cmake)


project(21kHz-Meta
    VERSION 0.1 
    DESCRIPTION "21kHz for MetaModule" 
    LANGUAGES C CXX ASM
)

add_library(21kHz STATIC)

# Add sources from the Rack source repo
set(SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/21kHz-rack-plugins/src)

target_sources(21kHz
    PRIVATE
    ${SOURCE_DIR}/21kHz.cpp
    ${SOURCE_DIR}/D_Inf.cpp
    ${SOURCE_DIR}/PalmLoop.cpp
    ${SOURCE_DIR}/TachyonEntangler.cpp
)

# Add includes and compile options for source repo
target_include_directories(21kHz
    PRIVATE
    ${SOURCE_DIR}
)

# Call this to link and create the plugin file
create_plugin(
    SOURCE_LIB      21kHz                                           # The cmake target name (defined in add_target)
    PLUGIN_NAME     21kHz                                           # This must match the brand "slug" used in VCV Rack
    PLUGIN_JSON     ${SOURCE_DIR}/../plugin.json                    # Path to the plugin.json file used by VCV Rack
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets                # Path to the assets/ dir containing the PNGs
    DESTINATION     ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins    # Path to where you want the plugin file output
)