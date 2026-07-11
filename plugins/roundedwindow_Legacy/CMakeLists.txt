set(CMAKE_CXX_STANDARD 20)

find_package(KF6CoreAddons REQUIRED)
find_package(KF6Config REQUIRED)
find_package(KF6WindowSystem REQUIRED)

find_path(EFFECTS_H kwineffects.h PATH_SUFFIXES kf6)

if (EFFECTS_H)
    include_directories(${EFFECTS_H})
else()
    message(STATUS "didnt find kwineffects.h, not building effects")
endif()

find_library(KWIN_EFFECTS NAMES kwineffects PATH_SUFFIXES kf6)
find_library(KWIN_GLUTILS NAMES kwinglutils PATH_SUFFIXES kf6)
find_library(OPENGL NAMES GL)

if (NOT KWIN_EFFECTS)
    message(STATUS "didnt find kwineffects lib, not building effects")
endif()

if (NOT KWIN_GLUTILS)
    message(STATUS "didnt find kwin glutils lib, not building effects")
endif()

if (NOT OPENGL)
    message(STATUS "didnt find opengl, not building effects")
endif()

if (NOT EFFECTS_H OR NOT KWIN_GLUTILS OR NOT KWIN_EFFECTS OR NOT OPENGL)
    message(FATAL_ERROR "cant continue")
endif()

add_library(roundedwindow MODULE
    main.cpp
    roundedwindow.cpp
    resources.qrc
)

target_include_directories(roundedwindow PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_link_libraries(roundedwindow
    PUBLIC
        Qt6::Core
        Qt6::Gui
    PRIVATE
        KF6::CoreAddons
        KF6::ConfigCore
        KF6::WindowSystem
)

install (TARGETS roundedwindow DESTINATION ${QT_PLUGINS_DIR}/kwin/effects/plugins)
