cmake_minimum_required(VERSION 2.8)
project(AntTweakBar)

include(FindOpenGL)
include(FindSDL)

set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

# Project includes
set(ATB_INCLUDE_DIR include)

# Standard Packages
find_package(OpenGL REQUIRED)
# find_package(GLUT)
find_library(GLFW_LIBRARY glfw)

include_directories( ${ATB_INCLUDE_DIR} 
                     ${OPENGL_INCLUDE_DIR})
#                    ${GLUT_INCLUDE_DIR})

if(APPLE)

   include_directories ( /System/Library/Frameworks )
   
   find_library(APPKIT_LIBRARY AppKit)
   
   message("Appkit: ${APPKIT_LIBRARY}")

   set(EXTRA_LIBS ${APPKIT_LIBRARY})

   set(CMAKE_CXX_FLAGS "-g -Wall -ObjC++ -fPIC -fno-strict-aliasing -D__PLACEMENT_NEW_INLINE")

   add_definitions(-D_MACOSX)
   
endif(APPLE)

set( SOURCES src/LoadOGL.cpp
             src/TwBar.cpp
             src/TwColors.cpp
             src/TwDirect3D9.cpp
             src/TwDirect3D10.cpp
             #src/TwDirect3D11.cpp
             src/TwEventSFML.cpp
             src/TwFonts.cpp
             src/TwMgr.cpp
             src/TwOpenGL.cpp
             #src/TwOpenGLCore.cpp
             src/TwPrecomp.cpp
             src/TwEventGLFW.c
             src/TwEventGLUT.c
             src/TwEventSDL.c
             src/TwEventSDL12.c 
             src/TwEventSDL13.c
             src/TwEventWin.c
             )

if(TW_STATIC)
  add_library(AntTweakBarStatic ${SOURCES})
  message("Extra libs: ${EXTRA_LIBS}")
  target_link_libraries(AntTweakBarStatic
    ${GLUT_LIBRARY} 
    ${OPENGL_LIBRARY} 
    ${EXTRA_LIBS}
  )
else()
  add_library(AntTweakBar SHARED ${SOURCES})
  message("Extra libs: ${EXTRA_LIBS}")
  target_link_libraries(AntTweakBar  ${GLUT_LIBRARY} 
                                     ${OPENGL_LIBRARY} 
                                     ${EXTRA_LIBS})
endif()


#install(TARGETS AntTweakBar LIBRARY DESTINATION .)

#add_subdirectory(examples)