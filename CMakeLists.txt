cmake_minimum_required(VERSION 3.20)

project(lingmo-platform VERSION 1.0.0 DESCRIPTION "Lingmo Platform Plugins" LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(plugins)

install(FILES config/kglobalshortcutsrc DESTINATION /etc/xdg)
install(FILES config/kwinrc DESTINATION /etc/xdg)
install(FILES config/kwinrulesrc DESTINATION /etc/xdg)

install(DIRECTORY scripts/linglauncher DESTINATION /usr/share/kwin/scripts)
install(DIRECTORY scripts/ling_squash DESTINATION /usr/share/kwin/effects)
install(DIRECTORY scripts/ling_scale DESTINATION /usr/share/kwin/effects)
install(DIRECTORY scripts/ling_popups DESTINATION /usr/share/kwin/effects)
install(DIRECTORY tabbox/ling_thumbnail DESTINATION /usr/share/kwin/tabbox)
