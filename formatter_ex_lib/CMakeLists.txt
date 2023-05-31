cmake_minimum_required(VERSION 3.22.1 FATAL_ERROR)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter)
add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/src/formatter_ex.cpp)
target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib/src)
target_link_libraries(formatter_ex_lib formatter_lib)
