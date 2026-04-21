cmake_minimum_required(VERSION 3.10)

project(ProfilerLib)

# Set compiler warning flags in a platform/compiler-friendly way.
# MSVC uses different warning flags than GCC/Clang, so configure them conditionally.
if(MSVC)
    # /W4 = high warning level, /WX = treat warnings as errors (maps roughly to -Werror)
    add_compile_options(/W4 /WX)
else()
    # For GCC/Clang
    add_compile_options(-Wall -Wextra -Wpedantic -Werror)
endif()

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

if (CMAKE_BUILD_TYPE MATCHES RELEASE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")
else ()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Og")
endif ()

add_library(profilerLib src/Profiler.cpp src/ScopeEvent.cpp src/ProfilerUtil.cpp src/TraceEventFile.cpp src/DurationEvent.cpp)
target_include_directories(profilerLib PUBLIC external include)
target_compile_features(profilerLib PUBLIC cxx_std_17)

option(BUILD_EXECUTABLES "build executable" OFF)
if (BUILD_EXECUTABLES)
    add_subdirectory(app)
endif ()
