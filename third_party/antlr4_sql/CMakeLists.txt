if(${AUTO_UPDATE_GRAMMAR})
        add_custom_command(
                OUTPUT
                ${CMAKE_CURRENT_SOURCE_DIR}/sql_lexer.cpp
                ${CMAKE_CURRENT_SOURCE_DIR}/sql_parser.cpp
                ${CMAKE_CURRENT_SOURCE_DIR}/include/sql_lexer.h
                ${CMAKE_CURRENT_SOURCE_DIR}/include/sql_parser.h
                COMMAND cmake -D ROOT_DIR=${PROJECT_SOURCE_DIR} -P generate_grammar.cmake
                DEPENDS
                ${PROJECT_SOURCE_DIR}/src/antlr4/Sql.g4
                ${PROJECT_SOURCE_DIR}/scripts/antlr4/generate_grammar.cmake
                WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/scripts/antlr4)
endif()

add_library(antlr4_sql
        STATIC
        sql_lexer.cpp
        sql_parser.cpp)

target_include_directories(antlr4_sql
        PRIVATE ../antlr4_runtime/src)

target_link_libraries(antlr4_sql PRIVATE antlr4_runtime)
