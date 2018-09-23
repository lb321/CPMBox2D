cmake_minimum_required(VERSION 2.6)

project(Box2D)
set(CPM_MODULE_NAME "Box2D")
set(CPM_LIB_TARGET_NAME ${CPM_MODULE_NAME})

if ((DEFINED CPM_DIR) AND (DEFINED CPM_UNIQUE_ID) AND (DEFINED CPM_TARGET_NAME))
    set(CPM_LIB_TARGET_NAME ${CPM_TARGET_NAME})
    set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CPM_DIR})
    include(CPM)
else()
    set (CPM_DIR "${CMAKE_CURRENT_BINARY_DIR}/cpm-packages" CACHE TYPE STRING)
    find_package(Git)
    if(NOT GIT_FOUND)
        message(FATAL_ERROR "CPM requires Git.")
    endif()
    if (NOT EXISTS ${CPM_DIR}/CPM.cmake)
        message(STATUS "Cloning repo (https://github.com/iauns/cpm)")
        execute_process(
                COMMAND "${GIT_EXECUTABLE}" clone https://github.com/iauns/cpm ${CPM_DIR}
                RESULT_VARIABLE error_code
                OUTPUT_QUIET ERROR_QUIET)
        if(error_code)
            message(FATAL_ERROR "CPM failed to get the hash for HEAD")
        endif()
    endif()
    include(${CPM_DIR}/CPM.cmake)
endif()

if(UNIX)
	set(BOX2D_INSTALL_BY_DEFAULT ON)
else(UNIX)
	set(BOX2D_INSTALL_BY_DEFAULT OFF)
endif(UNIX)

set(BOX2D_VERSION 2.3.0)
set(LIB_INSTALL_DIR lib${LIB_SUFFIX})

# The Box2D library.
add_subdirectory(Box2D)

# This call will ensure all include directories and definitions are present
# in the target. These correspond to the modules that we added above.
CPM_InitModule(${CPM_MODULE_NAME})

# Globbing has some downsides, but the advantages outweigh the
# disadvantages.
file (GLOB Sources
        "Box2D/Dynamics/*.cpp"
        "Box2D/Dynamics/*.h"
        "Box2D/Common/*.h"
        "Box2D/Common/*.cpp"
        "Box2D/Collision/*.h"
        "Box2D/Collision/*.cpp"
        "Box2D/Collision/Shapes/*.h"
        "Box2D/Collision/Shapes/*.cpp"
        "Box2D/Dynamics/*.h"
        "Box2D/Dynamics/*.cpp"
        "Box2D/Dynamics/Contacts/*.h"
        "Box2D/Dynamics/Contacts/*.cpp"
        "Box2D/Dynamics/Joints/*.h"
        "Box2D/Dynamics/Joints/*.cpp"
        "Box2D/Rope/*.h"
        "Box2D/Rope/*.cpp"
        )

#-----------------------------------------------------------------------
# Library setup
#-----------------------------------------------------------------------

# Build the library.
add_library(${CPM_LIB_TARGET_NAME} ${Sources})
target_link_libraries(${CPM_LIB_TARGET_NAME} ${CPM_LIBRARIES})
