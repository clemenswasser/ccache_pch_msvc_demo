cmake_minimum_required(VERSION 3.25)

project(ccache_pch_msvc_demo LANGUAGES CXX)

set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT Embedded CACHE INTERNAL "ccache only supports /Z7 debug info with MSVC")

find_program(CCACHE_PROGRAM ccache REQUIRED)
set(CCACHE_PCH_OPTIONS "sloppiness=pch_defines,time_macros,include_file_mtime,include_file_ctime")
set(CMAKE_CXX_COMPILER_LAUNCHER "${CCACHE_PROGRAM}" "${CCACHE_PCH_OPTIONS}")

add_executable(ccache_pch_msvc_demo main.cpp)
target_precompile_headers(ccache_pch_msvc_demo PRIVATE [[<stdio.h>]])
