# SPDX-FileCopyrightText: 2023 UnionTech Software Technology Co., Ltd.
#
# SPDX-License-Identifier: CC0-1.0

find_package(Qt5 REQUIRED COMPONENTS Core Widgets)
find_package(Dtk REQUIRED COMPONENTS Gui Widget)

file(GLOB SRCS "*.h" "*.cpp" "../../interfaces/*.h" "utils/*.h" "utils/*.cpp")

add_executable(trayplugin-loader
    main.cpp
    dockdbusproxy.cpp
    dockdbusproxy.h
    widgetplugin.h
    widgetplugin.cpp
    pluginitem.h
    pluginitem.cpp
    quickpluginitem.h
    quickpluginitem.cpp
    utils/setproctitle.h
    utils/setproctitle.cpp
    loader.qrc
)

target_include_directories(trayplugin-loader PUBLIC
    "utils/"
    "../../interfaces/"
    "../tray-wayland-integration/"
    "${CMAKE_BINARY_DIR}/"
)

target_link_libraries(trayplugin-loader PRIVATE
    dockpluginmanager-interface
    Qt5::Core
    Qt5::Widgets
    Dtk::Widget
    Dtk::Gui
)

install(TARGETS trayplugin-loader DESTINATION ${CMAKE_INSTALL_LIBEXECDIR})

#dconfig
dtk_add_config_meta_files(APPID org.deepin.dde.dock FILES ./configs/org.deepin.dde.dock.plugin.common.json)

#schemas
install(FILES configs/com.deepin.dde.control-center.gschema.xml DESTINATION ${CMAKE_INSTALL_DATADIR}/glib-2.0/schemas)
install(FILES configs/com.deepin.dde.dock.module.gschema.xml DESTINATION ${CMAKE_INSTALL_DATADIR}/glib-2.0/schemas)
