cmake_minimum_required(VERSION 3.19)
project(my_application LANGUAGES CXX)

find_package(SixtyFPS QUIET)
if (NOT SixtyFPS_FOUND)
  message("SixtyFPS could not be located in the CMake module search path. Downloading it from Git and building it locally")
  include(FetchContent)
  FetchContent_Declare(
    SixtyFPS
    GIT_REPOSITORY https://github.com/sixtyfpsui/sixtyfps.git
    GIT_TAG v0.1.5
    SOURCE_SUBDIR api/sixtyfps-cpp
  )
  FetchContent_MakeAvailable(SixtyFPS)
endif (NOT SixtyFPS_FOUND)

add_executable(my_application src/main.cpp)
target_link_libraries(my_application PRIVATE SixtyFPS::SixtyFPS)
sixtyfps_target_60_sources(my_application ui/appwindow.60)
