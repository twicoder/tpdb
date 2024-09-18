include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_subdirectory(main)

add_library(tpdb STATIC ${ALL_OBJECT_FILES})
add_library(tpdb_shared SHARED ${ALL_OBJECT_FILES})

set(TPDB_LIBRARIES antlr4_sql antlr4_runtime Threads::Threads)
if(NOT WIN32)
    set(TPDB_LIBRARIES dl ${TPDB_LIBRARIES})
endif()

# Seems to be needed for clang on linux only
# for compiling std::atomic<T>::compare_exchange_weak
if(NOT APPLE AND CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    set(TPDB_LIBRARIES atomic ${TPDB_LIBRARIES})
endif()

target_link_libraries(tpdb PUBLIC ${TPDB_LIBRARIES})
target_link_libraries(tpdb_shared PUBLIC ${TPDB_LIBRARIES})
unset(TPDB_LIBRARIES)

set(TPDB_INCLUDES $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}> ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(tpdb PUBLIC ${TPDB_INCLUDES})
target_include_directories(tpdb_shared PUBLIC ${TPDB_INCLUDES})
unset(TPDB_INCLUDES)

if(WIN32)
    # Anything linking against the static library must not use dllimport.
    target_compile_definitions(tpdb INTERFACE TPDB_STATIC_DEFINE)
endif()

if(NOT WIN32)
    set_target_properties(tpdb_shared PROPERTIES OUTPUT_NAME tpdb)
endif()

install(TARGETS tpdb tpdb_shared)
