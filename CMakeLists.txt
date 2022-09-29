cmake_minimum_required(VERSION 3.3 FATAL_ERROR)
project(kaldiio CXX)

set(KALDIIO_VERSION "1.15.1")

if(CMAKE_TOOLCHAIN_FILE)
  set(_BUILD_PYTHON OFF)
  set(_BUILD_TESTS OFF)
else()
  set(_BUILD_PYTHON ON)
  set(_BUILD_TESTS ON)
endif()

if(DEFINED _GLIBCXX_USE_CXX11_ABI)
  set(CMAKE_CXX_FLAGS " ${CMAKE_CXX_FLAGS} -D_GLIBCXX_USE_CXX11_ABI=${_GLIBCXX_USE_CXX11_ABI}")
endif()

# set(CMAKE_CXX_FLAGS " ${CMAKE_CXX_FLAGS} -Wall ")
if(CMAKE_COMPILER_IS_GNUCXX)
  # set(CMAKE_CXX_FLAGS " ${CMAKE_CXX_FLAGS} -Werror -Wextra -Wpedantic -pedantic-errors -g ")
endif()

set(ALLOWABLE_BUILD_TYPES Debug Release RelWithDebInfo MinSizeRel)
set(DEFAULT_BUILD_TYPE "Release")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "${ALLOWABLE_BUILD_TYPES}")
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  # CMAKE_CONFIGURATION_TYPES: with config type values from other generators (IDE).
  message(STATUS "No CMAKE_BUILD_TYPE given, default to ${DEFAULT_BUILD_TYPE}")
  set(CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}")
elseif(NOT CMAKE_BUILD_TYPE IN_LIST ALLOWABLE_BUILD_TYPES)
  message(FATAL_ERROR "Invalid build type: ${CMAKE_BUILD_TYPE}, \
    choose one from ${ALLOWABLE_BUILD_TYPES}")
endif()

message(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")

option(KALDI_NATIVE_IO_BUILD_TESTS "Whether to build tests or not" ${_BUILD_TESTS})
option(KALDI_NATIVE_IO_BUILD_PYTHON "Whether to build Python extension" ${_BUILD_PYTHON})

set(CMAKE_CXX_STANDARD 14 CACHE STRING "The C++ version to be used.")
set(BUILD_SHARED_LIBS ON)

if(WIN32)
  message(STATUS "Set BUILD_SHARED_LIBS to OFF for Windows")
  set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
endif()

if(WIN32)
  set(disabled_warnings
    /wd4018
    /wd4061
    /wd4127
    /wd4191
    /wd4242
    /wd4244
    /wd4267
    /wd4355
    /wd4365
    /wd4371
    /wd4456
    /wd4458
    /wd4464
    /wd4514
    /wd4623
    /wd4625
    /wd4626
    /wd4668
    /wd4686
    /wd4710
    /wd4711
    /wd4820
    /wd4868
    /wd4996
    /wd5027
    /wd5039
    /wd5045
    /wd5219
  )

  message(STATUS "Disabled warnings: ${disabled_warnings}")
  foreach(w IN LISTS disabled_warnings)
    string(APPEND CMAKE_CXX_FLAGS " ${w} ")
  endforeach()
endif()

message(STATUS "CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")

include(GNUInstallDirs)

message(STATUS "CMAKE_INSTALL_PREFIX: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")

add_subdirectory(kaldiio)
