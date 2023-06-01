cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_package)

include_directories("formatter_lib")
include_directories("formatter_ex_lib")
include_directories("solver_lib")

add_library(formatter_lib STATIC "formatter_lib/src/formatter.cpp")
add_library(formatter_ex_lib STATIC "formatter_ex_lib/src/formatter_ex.cpp")
add_library(solver_lib STATIC "solver_lib/src/solver.cpp")

add_executable(solver "solver_application/src/equation.cpp")

target_link_libraries(solver solver_lib formatter_ex_lib formatter_lib)

include(CPackConfig.cmake)
