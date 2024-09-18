add_library(tpdb_main
    OBJECT
    main.cpp)

set(ALL_OBJECT_FILES
    ${ALL_OBJECT_FILES} $<TARGET_OBJECTS:tpdb_main>
    PARENT_SCOPE)
