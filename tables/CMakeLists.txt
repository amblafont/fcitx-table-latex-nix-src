
set(TABLE_NAMES
  other/latex
  )

foreach(table ${TABLE_NAMES})
  fcitx_translate_add_sources("${table}.conf.in")
endforeach()


set(MB_FILES)
set(CONF_FILES)

foreach(table ${TABLE_NAMES})
  get_filename_component(table_name "${table}" NAME)
  set(mb_file "${CMAKE_CURRENT_BINARY_DIR}/${table_name}.mb")
  set(conf_file "${CMAKE_CURRENT_BINARY_DIR}/${table_name}.conf")

  fcitx_translate_add_apply_source("${table}.conf.in"
    "${conf_file}")
  get_filename_component(out_dir
    "${CMAKE_CURRENT_BINARY_DIR}/${table}.mb" PATH)
  add_custom_command(OUTPUT "${mb_file}"
    DEPENDS "${table}.txt" "${TXT2MB}"
    COMMAND "${CMAKE_COMMAND}" -E make_directory "${out_dir}"
    COMMAND "${TXT2MB}" ARGS "${CMAKE_CURRENT_SOURCE_DIR}/${table}.txt"
    "${CMAKE_CURRENT_BINARY_DIR}/${table_name}.mb")
  set(MB_FILES ${MB_FILES} "${mb_file}")
  set(CONF_FILES ${CONF_FILES} "${conf_file}")
endforeach()

add_custom_target(table_data ALL DEPENDS ${MB_FILES})

install(FILES ${CONF_FILES} DESTINATION "${FCITX4_PREFIX}/share/fcitx/table")
install(FILES ${MB_FILES} DESTINATION "${FCITX4_PREFIX}/share/fcitx/table")
