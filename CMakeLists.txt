cmake_minimum_required(VERSION 3.19)
project(my_application LANGUAGES CXX)

# Note: Use find_package(SixtyFPS) instead of the following three commands, if you prefer the package
# approach.
include(FetchContent)
FetchContent_Declare(
    SixtyFPS
    GIT_REPOSITORY https://github.com/sixtyfpsui/sixtyfps.git
    GIT_TAG v0.1.3
    SOURCE_SUBDIR api/sixtyfps-cpp
)
FetchContent_MakeAvailable(SixtyFPS)

add_executable(my_application src/main.cpp)
target_link_libraries(my_application PRIVATE SixtyFPS::SixtyFPS)
sixtyfps_target_60_sources(my_application ui/appwindow.60)
