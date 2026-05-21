cmake_minimum_required(VERSION 3.19)
project(Test VERSION 1.0 LANGUAGES CXX )

find_package(OpenCV REQUIRED)

add_subdirectory(common)
add_subdirectory(modules)

add_executable(CMaketest main.cpp )

target_link_libraries(CMaketest
 PRIVATE 
    M2
    M1
    A2
    A1
    kalman
    math
    ${OpenCV_LIBS}
)
