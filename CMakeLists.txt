cmake_minimum_required(VERSION 3.25)

project(ccache_pch_msvc_demo LANGUAGES CXX)

set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT Embedded CACHE INTERNAL "ccache only supports /Z7 debug info with MSVC")

find_program(CCACHE_PROGRAM ccache REQUIRED)
set(CCACHE_PCH_OPTIONS "sloppiness=pch_defines,time_macros,include_file_mtime,include_file_ctime")
set(CMAKE_CXX_COMPILER_LAUNCHER "${CCACHE_PROGRAM}" "${CCACHE_PCH_OPTIONS}")

add_executable(ccache_pch_msvc_demo main.cpp)
target_precompile_headers(ccache_pch_msvc_demo PRIVATE [[<stdio.h>]])

# Taken from: https://en.cppreference.com/w/cpp/header
set(CPP_HEADERS
    <algorithm>
    <any>
    <array>
    <atomic>
    <barrier>
    <bit>
    <bitset>
    <cassert>
    <cctype>
    <cerrno>
    <cfenv>
    <cfloat>
    <charconv>
    <chrono>
    <cinttypes>
    <climits>
    <clocale>
    <cmath>
    <codecvt>
    <compare>
    <complex>
    <concepts>
    <condition_variable>
    <coroutine>
    <csetjmp>
    <csignal>
    <cstdarg>
    <cstddef>
    <cstdint>
    <cstdio>
    <cstdlib>
    <cstring>
    <ctime>
    <cuchar>
    <cwchar>
    <cwctype>
    <deque>
    <exception>
    <execution>
    <expected>
    <filesystem>
    <format>
    <forward_list>
    <fstream>
    <functional>
    <future>
    <initializer_list>
    <iomanip>
    <ios>
    <iosfwd>
    <iostream>
    <istream>
    <iterator>
    <latch>
    <limits>
    <list>
    <locale>
    <map>
    <memory_resource>
    <memory>
    <mutex>
    <new>
    <numbers>
    <numeric>
    <optional>
    <ostream>
    <queue>
    <random>
    <ranges>
    <ratio>
    <regex>
    <scoped_allocator>
    <semaphore>
    <set>
    <shared_mutex>
    <source_location>
    <span>
    <spanstream>
    <sstream>
    <stack>
    #<stacktrace>
    <stdexcept>
    <stdfloat>
    <stop_token>
    <streambuf>
    <string_view>
    <string>
    <syncstream>
    <system_error>
    <thread>
    <tuple>
    <type_traits>
    <typeindex>
    <typeinfo>
    <unordered_map>
    <unordered_set>
    <utility>
    <valarray>
    <variant>
    <vector>
    <version>)

list(LENGTH CPP_HEADERS CPP_HEADERS_LENGTH)
set(CPP_HEADERS_IDX 1)
while(CPP_HEADERS_IDX LESS CPP_HEADERS_LENGTH)
    list(SUBLIST CPP_HEADERS 0 "${CPP_HEADERS_IDX}" "CPP_HEADERS_${CPP_HEADERS_IDX}")

    add_library("pch_bench_${CPP_HEADERS_IDX}" empty.cpp)
    target_precompile_headers("pch_bench_${CPP_HEADERS_IDX}" PRIVATE "${CPP_HEADERS_${CPP_HEADERS_IDX}}")
    target_compile_features("pch_bench_${CPP_HEADERS_IDX}" PRIVATE cxx_std_23)

    math(EXPR CPP_HEADERS_IDX "${CPP_HEADERS_IDX}+1")
endwhile()
