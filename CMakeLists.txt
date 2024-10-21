cmake_minimum_required(VERSION 3.10)
project(spheretree-dist VERSION 1.0 LANGUAGES CXX C)

# Set the required C/C++ standards
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -pthread")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
set(CMAKE_LINKER_FLAGS "${CMAKE_LINKER_FLAGS}")
set(CMAKE_LINKER_FLAGS_DEBUG "${CMAKE_LINKER_FLAGS_DEBUG}")

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-int-in-bool-context -Wsign-compare")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fsanitize=address")
    set(CMAKE_LINKER_FLAGS "${CMAKE_LINKER_FLAGS}")
    set(CMAKE_LINKER_FLAGS_DEBUG "${CMAKE_LINKER_FLAGS_DEBUG} -fsanitize=address")
endif()


SET(CMAKE_BUILD_TYPE "Release")
SET(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O3 -Wall -fPIC -DNDEBUG")

# Include all subdirectories that are part of the project
add_subdirectory(API)
add_subdirectory(Base)
add_subdirectory(BBox)
add_subdirectory(Exceptions)
add_subdirectory(Geometry)
add_subdirectory(LinearAlgebra)
add_subdirectory(MedialAxis)
add_subdirectory(MinMax)
add_subdirectory(SphereTree)
add_subdirectory(Storage)
add_subdirectory(Surface)
add_subdirectory(Export)

# Add the static library equivalent to libSTG.la
add_library(STG STATIC
        null.c
)

# Link dependent libraries to libSTG
target_link_libraries(STG
        API
        Base
        BBox
        Exceptions
        Geometry
        Matrix
        MedialAxis
        MinMax
        SphereTree
        Storage
        Surface
        Export
)

# Define the list of programs to be built
set(PROGRAMS
        makeTreeMedial
        makeTreeHubbard
        makeTreeOctree
        makeTreeGrid
        makeTreeSpawn
)

# Add each program and its respective source files
foreach(PROGRAM ${PROGRAMS})
    if(${PROGRAM} STREQUAL "makeTreeMedial")
        add_executable(${PROGRAM}
                makeTreeMedial.cpp
                DecodeParam.cpp
                DecodeParam.h
                EvalTree.cpp
                EvalTree.h
                VerifyModel.cpp
                VerifyModel.h
        )
    elseif(${PROGRAM} STREQUAL "makeTreeHubbard")
        add_executable(${PROGRAM}
                makeTreeHubbard.cpp
                VerifyModel.cpp
                VerifyModel.h
                DecodeParam.cpp
                DecodeParam.h
        )
    elseif(${PROGRAM} STREQUAL "makeTreeOctree")
        add_executable(${PROGRAM}
                makeTreeOctree.cpp
                DecodeParam.cpp
                DecodeParam.h
        )
    elseif(${PROGRAM} STREQUAL "makeTreeGrid")
        add_executable(${PROGRAM}
                makeTreeGrid.cpp
                VerifyModel.cpp
                VerifyModel.h
                EvalTree.cpp
                EvalTree.h
                DecodeParam.cpp
                DecodeParam.h
        )
    elseif(${PROGRAM} STREQUAL "makeTreeSpawn")
        add_executable(${PROGRAM}
                makeTreeSpawn.cpp
                VerifyModel.cpp
                VerifyModel.h
                EvalTree.cpp
                EvalTree.h
                DecodeParam.cpp
                DecodeParam.h
        )
    endif()

    # Link the libSTG static library to each executable
    target_link_libraries(${PROGRAM} STG)
endforeach()

# Installation rules for executables
install(TARGETS ${PROGRAMS} RUNTIME DESTINATION bin)
