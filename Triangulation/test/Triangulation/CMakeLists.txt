# Created by the script cgal_create_cmake_script
# This is the CMake script for compiling a CGAL application.

cmake_minimum_required(VERSION 3.1...3.22)
project(Triangulation_Tests)

if(CMAKE_COMPILER_IS_GNUCCX AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.4)
  message(
    STATUS
      "NOTICE: this directory requires a version of gcc >= 4.4, and will not be compiled."
  )
  return()
endif()

find_package(CGAL REQUIRED)

find_package(Eigen3 3.1.0)
include(CGAL_Eigen3_support)
if(TARGET CGAL::Eigen3_support)
  include_directories(BEFORE "include")

  create_single_source_cgal_program("test_triangulation.cpp")
  create_single_source_cgal_program("test_delaunay.cpp")
  create_single_source_cgal_program("test_regular.cpp")
  create_single_source_cgal_program("test_tds.cpp")
  create_single_source_cgal_program("test_torture.cpp")
  create_single_source_cgal_program("test_insert_if_in_star.cpp")
  foreach(target test_triangulation test_delaunay test_regular test_tds
                 test_torture test_insert_if_in_star)
    target_link_libraries(${target} PUBLIC CGAL::Eigen3_support)
  endforeach()

else()
  message(
    STATUS
      "NOTICE: Some of the executables in this directory need Eigen 3.1 (or greater) and will not be compiled."
  )
endif()
