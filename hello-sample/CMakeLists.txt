cmake_minimum_required(VERSION 3.15)
project(hello CXX)

add_executable(hello src/hello.cpp src/main.cpp)
target_include_directories(hello PUBLIC include)

install(TARGETS hello)
