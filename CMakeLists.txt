cmake_minimum_required(VERSION 3.1.2)
project(coverage_mission_planner)


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

add_executable(${PROJECT_NAME} src/main.cpp src/MapPolygon.cpp include/MapPolygon.hpp src/utils.cpp src/algorithms.cpp src/EnergyCalculator.cpp src/ShortestPathCalculator.cpp include/ShortestPathCalculator.hpp include/custom_types.hpp include/mstsp_solver/Target.h src/mstsp_solver/TargetSet.cpp include/mstsp_solver/TargetSet.h include/mstsp_solver/SolverConfig.h src/mstsp_solver/MstspSolver.cpp include/mstsp_solver/MstspSolver.h include/mstsp_solver/Insertion.h include/SimpleLogger.h include/LoggerRos.h)

if(MSVC)
  target_compile_options(${PROJECT_NAME} PRIVATE /W4 /WX /D_USE_MATH_DEFINES)
else()
  target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -pedantic -Werror)
endif()

include(FetchContent)

FetchContent_Declare(
  yaml-cpp
  GIT_REPOSITORY https://github.com/jbeder/yaml-cpp.git
  GIT_TAG 0.8.0
)
FetchContent_GetProperties(yaml-cpp)

if(NOT yaml-cpp_POPULATED)
  message(STATUS "Fetching yaml-cpp...")
  FetchContent_Populate(yaml-cpp)
  add_subdirectory(${yaml-cpp_SOURCE_DIR} ${yaml-cpp_BINARY_DIR})
endif()


target_link_libraries(${PROJECT_NAME} PRIVATE yaml-cpp::yaml-cpp)
target_include_directories(${PROJECT_NAME} PRIVATE include)