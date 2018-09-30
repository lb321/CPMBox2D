cmake_minimum_required(VERSION 2.6)

project(Box2D)

if(UNIX)
	set(BOX2D_INSTALL_BY_DEFAULT ON)
else(UNIX)
	set(BOX2D_INSTALL_BY_DEFAULT OFF)
endif(UNIX)

set(BOX2D_VERSION 2.3.0)
set(LIB_INSTALL_DIR lib${LIB_SUFFIX})

# The Box2D library.
add_subdirectory(Box2D)

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

# Build the library.
add_library(Box2D ${Sources})
target_link_libraries(Box2D Box2D)
