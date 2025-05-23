add_fcitx5_addon(clipboard)

target_sources(clipboard PRIVATE clipboard.cpp)
target_link_libraries(clipboard Fcitx5::Core)

if (ENABLE_X11)
    target_sources(clipboard PRIVATE xcbclipboard.cpp)
    target_link_libraries(clipboard Fcitx5::Module::XCB XCB::XCB)
endif()

if (WAYLAND_FOUND)
    target_sources(clipboard PRIVATE waylandclipboard.cpp)
    target_link_libraries(clipboard Fcitx5::Module::Wayland Fcitx5::Wayland::WLRDataControl Pthread::Pthread)
endif()

install(TARGETS clipboard DESTINATION "${FCITX_INSTALL_ADDONDIR}")
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/clipboard.conf" DESTINATION "${FCITX_INSTALL_PKGDATADIR}/addon"
        COMPONENT config)
configure_file(clipboard.conf.in.in clipboard.conf.in @ONLY)
fcitx5_translate_desktop_file(${CMAKE_CURRENT_BINARY_DIR}/clipboard.conf.in clipboard.conf)
fcitx5_export_module(Clipboard TARGET clipboard BUILD_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}" HEADERS clipboard_public.h INSTALL)

