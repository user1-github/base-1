cmake_minimum_required(VERSION 4.0)

project(Blue_Nebula LANGUAGES CXX)

find_package(PkgConfig REQUIRED)
pkg_check_modules(DEPS REQUIRED IMPORTED_TARGET gl zlib libenet sdl2 SDL2_image SDL2_mixer)

file(GLOB CLIENT_SOURCES engine/*.cpp game/*.cpp shared/*.cpp)
file(GLOB EXCLUDED_SOURCES engine/genkey.cpp)
list(REMOVE_ITEM CLIENT_SOURCES ${EXCLUDED_SOURCES})
add_executable(client ${CLIENT_SOURCES})

target_include_directories(client PRIVATE engine game shared)

target_link_libraries(client PRIVATE PkgConfig::DEPS)

target_compile_options(client PRIVATE -O3 -fomit-frame-pointer -ffast-math -Wall -fsigned-char -fno-exceptions -fno-rtti)
