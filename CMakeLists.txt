cmake_minimum_required(VERSION 3.16)
project(asssgt3 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(asssgt3 main.cpp)

if(WIN32)
    target_include_directories(asssgt3 PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/include")
    target_link_libraries(asssgt3 PRIVATE
        "${CMAKE_CURRENT_SOURCE_DIR}/lib/freeglut.lib"
        opengl32
        glu32
    )

    # Keep the bundled FreeGLUT runtime beside the executable.
    add_custom_command(TARGET asssgt3 POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${CMAKE_CURRENT_SOURCE_DIR}/freeglut.dll"
            "$<TARGET_FILE_DIR:asssgt3>/freeglut.dll"
    )
elseif(APPLE)
    find_package(OpenGL REQUIRED)
    find_package(GLUT REQUIRED)
    target_link_libraries(asssgt3 PRIVATE OpenGL::GL GLUT::GLUT)
else()
    find_package(OpenGL REQUIRED)
    find_package(GLUT REQUIRED)
    target_link_libraries(asssgt3 PRIVATE OpenGL::GL OpenGL::GLU GLUT::GLUT)
endif()
