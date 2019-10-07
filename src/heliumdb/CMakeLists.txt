set (SOURCES
     module.cpp
     utils.cpp
     iter.cpp
    )

Python_add_library (heliumdb MODULE ${SOURCES})
target_link_libraries(heliumdb ${LIBHE})

install (TARGETS heliumdb DESTINATION src/heliumdb)
