cmake_minimum_required(VERSION 3.15)

# module

project(socket_core LANGUAGES C)

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS 1)

file(GLOB SRC_FILES src/*.c)
file(GLOB HEADER_FILES include/*.h)

if(WIN32)
    list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/serial.c")
    list(REMOVE_ITEM HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/serial.h")
    list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/usocket.c")
    list(REMOVE_ITEM HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/usocket.h")
    list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/unixdgram.c")
    list(REMOVE_ITEM HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/unixdgram.h")
    list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/unixstream.c")
    list(REMOVE_ITEM HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/unixstream.h")
    list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/unix.c")
    list(REMOVE_ITEM HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/unix.h")
    list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/mime.c")
    list(REMOVE_ITEM HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/mime.h")
elseif(UNIX AND NOT APPLE)
endif()

add_library(socket_core SHARED ${SRC_FILES} ${HEADER_FILES})
target_include_directories(socket_core PUBLIC include)

target_include_directories(socket_core PUBLIC deps/module-sdk/include)
target_compile_features(socket_core PRIVATE cxx_std_17)
set_target_properties(socket_core PROPERTIES LINKER_LANGUAGE C)

if(WIN32)
    target_link_libraries(socket_core PRIVATE "${CMAKE_SOURCE_DIR}/deps/module-sdk/lib/lua.lib" "ws2_32")
elseif(UNIX AND NOT APPLE)
    target_link_libraries(socket_core PRIVATE "${CMAKE_SOURCE_DIR}/deps/module-sdk/lib/liblua.a")
endif()
