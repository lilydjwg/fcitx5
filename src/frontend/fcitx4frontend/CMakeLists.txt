add_fcitx5_addon(fcitx4frontend fcitx4frontend.cpp)
target_link_libraries(fcitx4frontend Fcitx5::Core Fcitx5::Module::DBus)
if (ENABLE_X11)
    target_link_libraries(fcitx4frontend Fcitx5::Module::XCB XCB::XCB)
endif()
install(TARGETS fcitx4frontend DESTINATION "${FCITX_INSTALL_ADDONDIR}")
configure_file(fcitx4frontend.conf.in.in fcitx4frontend.conf.in @ONLY)
fcitx5_translate_desktop_file(${CMAKE_CURRENT_BINARY_DIR}/fcitx4frontend.conf.in fcitx4frontend.conf)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/fcitx4frontend.conf" DESTINATION "${FCITX_INSTALL_PKGDATADIR}/addon"
        COMPONENT config)
