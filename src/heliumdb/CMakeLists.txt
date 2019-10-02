set (SOURCES
     module.cpp
     utils.cpp
     iter.cpp
    )

Python_add_library (heliumdb MODULE ${SOURCES})
target_link_libraries(heliumdb ${LIBHE})
set_target_properties (heliumdb PROPERTIES PREFIX "")
set_target_properties (heliumdb PROPERTIES SUFFIX ".so")

install (TARGETS heliumdb DESTINATION src/heliumdb)
