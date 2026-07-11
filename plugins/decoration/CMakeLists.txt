project (lingmo-decoration VERSION 1.0.0 DESCRIPTION "Lingmo Window Decoration" LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 20)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

find_package(KF6CoreAddons QUIET)
find_package(KF6Config QUIET)
find_package(KF6WindowSystem QUIET)
find_package(KDecoration2 QUIET)
if(NOT KF6WindowSystem_FOUND OR NOT KDecoration2_FOUND)
    message(STATUS "KF6WindowSystem or KDecoration2 not found, skipping lingmo-decoration")
    return()
endif()

message(STATUS "KDecoration2 version: ${KDecoration2_VERSION}")
string(REPLACE "." ";" version_parts ${KDecoration2_VERSION})
list(GET version_parts 0 KDECORATION_MAJOR)
list(GET version_parts 1 KDECORATION_MINOR)
list(GET version_parts 2 KDECORATION_PATCH)
add_compile_definitions(KDECORATION_MAJOR=${KDECORATION_MAJOR})
add_compile_definitions(KDECORATION_MINOR=${KDECORATION_MINOR})
add_compile_definitions(KDECORATION_PATCH=${KDECORATION_PATCH})

find_package(Qt6 CONFIG REQUIRED COMPONENTS Gui Widgets Core)

get_target_property(QT_QMAKE_EXECUTABLE ${Qt6Core_QMAKE_EXECUTABLE} IMPORTED_LOCATION)
if(NOT QT_QMAKE_EXECUTABLE)
    message(FATAL_ERROR "qmake is not found.")
endif()

execute_process(COMMAND ${QT_QMAKE_EXECUTABLE} -query QT_INSTALL_PLUGINS
        OUTPUT_VARIABLE QT_PLUGINS_DIR
        OUTPUT_STRIP_TRAILING_WHITESPACE
)
if(QT_PLUGINS_DIR)
    message(STATUS "Qt6 plugin directory: ${QT_PLUGINS_DIR}")
else()
    message(FATAL_ERROR "Qt6 plugin directory cannot be detected.")
endif()

set(decoration_SRCS
    decoration.cpp
    x11shadow.cpp
    button.cpp
    resources.qrc
)

add_library(lingmo-decoration MODULE
    ${decoration_SRCS}
)

target_link_libraries(lingmo-decoration
    PUBLIC
        Qt6::Core
        Qt6::Gui
        Qt6::Widgets
        KF6::ConfigCore
        KF6::ConfigGui
        KF6::CoreAddons
        KF6::WindowSystem

    PRIVATE
        KDecoration2::KDecoration
)

install(TARGETS lingmo-decoration
         DESTINATION ${QT_PLUGINS_DIR}/org.kde.kdecoration2)
