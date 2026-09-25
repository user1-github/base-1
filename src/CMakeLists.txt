cmake_minimum_required(VERSION 3.5)

# set compile options
set(CMAKE_CXX_FLAGS "-O3 -fomit-frame-pointer -ffast-math -Wall -fsigned-char -fno-rtti")

# Use pkg-config to configure dependencies later
find_package(PkgConfig REQUIRED)
# the dependencies required by all platforms
set(client_deps zlib sdl2 SDL2_image SDL2_mixer gl libenet)

# the client depends on almost all the source files
file(GLOB client_sources engine/*.cpp game/*.cpp shared/*.cpp)

# neither server nor client need genkey.cpp
# to avoid warnings about duplicate main()s, it has to be removed from their source lists
file(GLOB genkey_cpp_path engine/genkey.cpp)
list(REMOVE_ITEM client_sources ${genkey_cpp_path})

# platform specific code
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    link_libraries(rt)
    set(BIN_SUFFIX "_linux")
endif()

# configure dependencies with pkg-config
foreach(dep IN LISTS client_deps)
    pkg_check_modules(${dep} REQUIRED ${dep})

    # add the necessary includes
    foreach(include_dir IN LISTS ${dep}_INCLUDE_DIRS)
        include_directories(${include_dir})
    endforeach(include_dir)

    # link to the libraries
    foreach(lib IN LISTS ${dep}_LIBRARIES)
        link_libraries(${lib})
    endforeach(lib)

    # tell the compiler where to find the libraries' binaries
    foreach(lib_dir IN LISTS ${dep}_LIBRARY_DIRS)
        link_directories(${lib_dir})
    endforeach(lib_dir)
endforeach(dep)

# configure local includes
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/engine
    ${CMAKE_CURRENT_SOURCE_DIR}/game
    ${CMAKE_CURRENT_SOURCE_DIR}/shared
)

# add the client executable
add_executable(blue-nebula${BIN_SUFFIX} ${client_sources})
