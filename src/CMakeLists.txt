cmake_minimum_required(VERSION 4.0)

project(Blue_Nebula LANGUAGES CXX)

set(CMAKE_CXX_FLAGS "-O3 -fomit-frame-pointer -ffast-math -Wall -fsigned-char -fno-exceptions -fno-rtti")

find_package(SDL2 REQUIRED)
find_package(SDL2_image REQUIRED)
find_package(SDL2_mixer REQUIRED)
find_package(OpenGL REQUIRED)
find_package(ZLIB REQUIRED)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ENET REQUIRED IMPORTED_TARGET libenet)

file(GLOB CLIENT_SOURCES engine/*.cpp game/*.cpp shared/*.cpp)
file(GLOB EXCLUDED_SOURCES engine/genkey.cpp)
list(REMOVE_ITEM CLIENT_SOURCES ${EXCLUDED_SOURCES})
add_executable(client ${CLIENT_SOURCES})

target_include_directories(client PRIVATE engine game shared)

target_link_libraries(client PRIVATE SDL2::SDL2 SDL2_image::SDL2_image SDL2_mixer::SDL2_mixer OpenGL::OpenGL ZLIB::ZLIB PkgConfig::ENET)
