cmake_minimum_required(VERSION 3.10...3.15)
project(MassCalculator
        VERSION 0.1
        DESCRIPTION "MassCalculator app"
        LANGUAGES CXX
)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
                      /usr/local/share/cmake
                      /usr/share/cmake)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

#-Wmissing-include-dirs
set(CMAKE_CXX_FLAGS 
    "-O2 \
    -pthread \
    -pedantic \
    -Wall \
    -Wextra \
    -Wcast-align \
    -Wcast-qual \
    -Wctor-dtor-privacy \
    -Wdisabled-optimization \
    -Wformat=2 \
    -Winit-self \
    -Wlogical-op \
    -Wmissing-declarations \
    -Wnoexcept \
    -Wold-style-cast \
    -Woverloaded-virtual \
    -Wredundant-decls \
    -Wshadow \
    -Wsign-conversion \
    -Wsign-promo \
    -Wstrict-null-sentinel \
    -Wstrict-overflow=5 \
    -Wswitch-default \
    -Wundef \
    -Werror \
    -Wno-unused"
)

find_package(GTest REQUIRED >= 1.8)

# Only do these if this is the main project, and not if it is included through add_subdirectory
if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)

    # Optionally set things like CMAKE_CXX_STANDARD, CMAKE_POSITION_INDEPENDENT_CODE here
    
    # Let's ensure -std=c++xx instead of -std=g++xx
    set(CMAKE_CXX_EXTENSIONS OFF)

    # Let's nicely support folders in IDE's
    set_property(GLOBAL PROPERTY USE_FOLDERS ON)

    # Testing only available if this is the main app
    # Note this needs to be done in the main CMakeLists
    # since it calls enable_testing, which must be in the
    # main CMakeLists.
    include(CTest)

    # Docs only available if this is the main app
    find_package(Doxygen)
    if(Doxygen_FOUND)
	add_subdirectory(docs)
    else()
	message(STATUS "Doxygen not found, not building docs")
    endif()
endif()

# add_compile_definitions(ROOT_PATH=${CMAKE_CURRENT_SOURCE_DIR} DEBUG)

add_subdirectory(libmasscalculator)
# add_subdirectory(cli)
# add_subdirectory(gui)
# add_subdirectory(docs)
# add_subdirectory(docker)

# ###############
# ## Packaging ##
# ###############
# #

# # Done individualy in the library and in the GUI

# # TODO refactor cmake config to be "modern cmake":
# # https://gist.github.com/mbinna/c61dbb39bca0e4fb7d1f73b0d66a4fd1