cmake_minimum_required(VERSION 3.1.0)

# set(CMAKE_SYSTEM_NAME Linux)
# set(CMAKE_C_COMPILER /usr/bin/arm-linux-gnueabi-gcc)
# set(CMAKE_CXX_COMPILER /usr/bin/arm-linux-gnueabi-g++)

set(CMAKE_CXX_STANDARD 14)  # Cannot use 17+...
# message("Compiling with c++${CMAKE_CXX_STANDARD}.")

project(EV3_SLAM)
add_compile_options(-static)
add_compile_options(-Wall -Wextra -pedantic -Werror)

# N.B. This project requires the ev3dev-lang-cpp library: https://github.com/ddemidov/ev3dev-lang-cpp.
add_library(ev3dev ev3dev-lang-cpp/ev3dev.cpp)
target_include_directories(ev3dev PUBLIC ev3dev-lang-cpp/)

add_subdirectory(Report)
add_subdirectory(Hardware)
add_subdirectory(SparseGSLAM)

add_executable(ev3slam main.cpp)
target_link_libraries(ev3slam PRIVATE ev3dev hardware-scope hardware report)

# TODO: Why `/usr/arm-linux-gnueabi/`?

# Need to write in terminal (added to Dockerfile...):
# ` export QEMU_LD_PREFIX=/usr/arm-linux-gnueabi `

# How to build EV3DEV cpp lang lib (TODO: Add to Dockerfile)
#   mkdir build
#   cd build
#   cmake .. -DEV3DEV_PLATFORM=EV3
#   make
