cmake_minimum_required(VERSION 4.0)

project("Blue Nebula" LANGUAGES C CXX)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_C_FLAGS "-fomit-frame-pointer")
set(CMAKE_CXX_FLAGS "-fomit-frame-pointer -ffast-math -Wall -fsigned-char -fexceptions -fno-rtti")

find_package(ZLIB REQUIRED)
find_package(SDL2 REQUIRED)
find_package(SDL2_image REQUIRED)
find_package(SDL2_mixer REQUIRED)
find_package(OpenGL REQUIRED)

add_subdirectory(enet EXCLUDE_FROM_ALL)

file(GLOB CLIENT_SOURCES engine/*.cpp game/*.cpp shared/*.cpp)
file(GLOB EXCLUDED_SOURCES engine/genkey.cpp)
list(REMOVE_ITEM CLIENT_SOURCES ${EXCLUDED_SOURCES})
add_executable(client ${CLIENT_SOURCES})

target_include_directories(client PRIVATE enet/include engine game shared)

target_link_libraries(client PRIVATE ZLIB::ZLIB SDL2::SDL2 SDL2_image::SDL2_image SDL2_mixer::SDL2_mixer OpenGL::OpenGL enet rt)
