cmake_minimum_required(VERSION 2.6)

set(PLUGIN "analogclock")


set(HW_HEADERS
  clock.h
  configuredialog.h
)

set(HW_SOURCES
  clock.cpp
  configuredialog.cpp
)

set(HW_MOCS
    configuredialog.h
)

set(AC_UIS
  configuredialog.ui
)

set(RESOURCES
  resources.qrc
)
#*******************************************
set(PROJECT "${PLUGIN}")

project(${PROJECT})

set(PROGRAM "razor-desktop")

set(PROG_SHARE_DIR ${CMAKE_INSTALL_PREFIX}/share/razor/${PROGRAM})
set(PLUGIN_SHARE_DIR ${PROG_SHARE_DIR}/${PLUGIN})
file (GLOB TS_FILES         translations/*.ts   )

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../../libraries
    ${CMAKE_CURRENT_SOURCE_DIR}/../
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

qt4_wrap_cpp(HW_MOC_SOURCES ${HW_MOCS})
qt4_add_resources(QRC_SOURCES ${RESOURCES})
qt4_wrap_ui(FORMS ${AC_UIS})
qt4_add_translation(QM_FILES ${TS_FILES})

add_library(${PROJECT} SHARED ${HW_HEADERS} ${HW_SOURCES} ${HW_MOC_SOURCES} ${FORMS} ${QM_FILES} ${QRC_SOURCES})
add_dependencies(${PROJECT} razorqt)
target_link_libraries(${PROJECT} ${QT_QTCORE_LIBRARY} ${QT_QTGUI_LIBRARY} )

install(TARGETS ${PROJECT} DESTINATION ${DESKTOP_PLUGIN_DIR})
install(DIRECTORY resources/ DESTINATION share/razor/razor-desktop/ PATTERN ".svn" EXCLUDE)
install(FILES ${QM_FILES}      DESTINATION ${PLUGIN_SHARE_DIR})
