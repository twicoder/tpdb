if(NOT MSVC)
    add_compile_options(-Wno-extra)
endif()

add_subdirectory(antlr4_sql)
add_subdirectory(antlr4_runtime)

