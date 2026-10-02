cmake_minimum_required(VERSION 4.3)
project(asssgt3)

set(CMAKE_CXX_STANDARD 14)

include_directories(include)
include_directories(include/GL)

add_executable(asssgt3
        include/GL/freeglut.h
        include/GL/freeglut_ext.h
        include/GL/freeglut_std.h
        include/GL/glut.h
        lib/freeglut.lib
        freeglut.dll
        main.cpp)
