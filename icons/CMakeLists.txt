set(ICON48 fcitx-latex.png )


foreach(icon ${ICON48})
    string(REPLACE "fcitx-" "" icon_new ${icon})
    install(FILES ${icon} RENAME ${icon_new} DESTINATION share/fcitx/imicon)
endforeach()

install(FILES
    ${ICON48}
    DESTINATION share/icons/hicolor/48x48/apps)
