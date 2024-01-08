cmake_minimum_required(VERSION 3.17.0)
project(adapter_WT_patch_sqlcipher VERSION "0.0.1")

include(FetchContent)
set(FETCHCONTENT_QUIET OFF)
set(FETCHCONTENT_UPDATES_DISCONNECTED ON)



set(WT_SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/wt-src)
set(WT_BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/wt-build)
set(WT_INSTALL_DIR ${CMAKE_INSTALL_PREFIX})
set(WT_INCLUDE_DIR ${WT_INSTALL_DIR}/include)

set(WT_PATCH_SQLCIPHER_REPOSITORY "https://github.com/tipi-deps/wt" CACHE STRING "poco source repository")
set(WT_PATCH_SQLCIPHER_GIT_TAG "ae6801e8d0cf85bd5fe85dc3d6b89a3399a03223" CACHE STRING "poco release tag to consume")

MESSAGE(STATUS "Retrieving wt (release: ${WT_PATCH_SQLCIPHER_GIT_TAG}) from ${WT_PATCH_SQLCIPHER_REPOSITORY}")

find_package(OpenSSL REQUIRED)

add_link_options(-ldl)

add_compile_definitions(
    _GNU_SOURCE=1
    SQLITE_HAS_CODEC=1
    SQLITE_TEMP_STORE=3
    SQLCIPHER_CRYPTO_OPENSSL
)

include(FetchContent)
FetchContent_Declare(wt
    GIT_REPOSITORY    ${WT_PATCH_SQLCIPHER_REPOSITORY}
    GIT_TAG           ${WT_PATCH_SQLCIPHER_GIT_TAG}
    SOURCE_DIR        ${WT_SOURCE_DIR}
    SUBBUILD_DIR      ${WT_BUILD_DIR}
    PATCH_COMMAND     cp ${CMAKE_CURRENT_LIST_DIR}/sqlcipher/sqlite3.c ${CMAKE_CURRENT_LIST_DIR}/sqlcipher/sqlite3.h ${WT_SOURCE_DIR}/src/Wt/Dbo/backend/amalgamation/
)

FetchContent_MakeAvailable(wt)

file(MAKE_DIRECTORY ${WT_INSTALL_DIR}/bin/)

install(DIRECTORY DESTINATION ${WT_INSTALL_DIR}/adapter-wt-patch-sqlcipher.done)