if(MSVC)
    add_definitions(-DHAVE_ACOSH -DHAVE_ASINH -DHAVE_ATANH)
else(MSVC)
    add_definitions(-DHAVE_LIMITS_H -DHAVE_CONFIG_H)
endif(MSVC)

include_directories(
    ${CMAKE_BINARY_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${Boost_INCLUDE_DIRS}
    ${OCC_INCLUDE_DIR}
    ${QT_QTCORE_INCLUDE_DIR}
    ${PYTHON_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIR}
    ${XercesC_INCLUDE_DIRS}
)

link_directories(${OCC_LIBRARY_DIR})

set(Inspection_LIBS
    FreeCADApp
    Mesh
    Points
    Part
)

SET(Inspection_SRCS
    AppInspection.cpp
    AppInspectionPy.cpp
    InspectionFeature.cpp
    InspectionFeature.h
    PreCompiled.cpp
    PreCompiled.h
)

add_library(Inspection SHARED ${Inspection_SRCS})
target_link_libraries(Inspection ${Inspection_LIBS})


fc_target_copy_resource(Inspection 
    ${CMAKE_SOURCE_DIR}/src/Mod/Inspection
    ${CMAKE_BINARY_DIR}/Mod/Inspection
    Init.py)

SET_BIN_DIR(Inspection Inspection /Mod/Inspection)
SET_PYTHON_PREFIX_SUFFIX(Inspection)

INSTALL(TARGETS Inspection DESTINATION ${CMAKE_INSTALL_LIBDIR})
