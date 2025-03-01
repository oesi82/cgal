# Created by the script cgal_create_CMakeLists
# This is the CMake script for compiling a set of CGAL applications.

project(Polygonal_surface_reconstruction_Tests)

cmake_minimum_required(VERSION 3.1...3.22)

# CGAL and its components
find_package(CGAL REQUIRED)

# Boost and its components
find_package(Boost REQUIRED)

if(NOT Boost_FOUND)

  message(
    STATUS
      "NOTICE: This project requires the Boost library, and will not be compiled."
  )

  return()

endif()

# Creating entries for all C++ files with "main" routine
# ##########################################################

find_package(Eigen3 3.1.0) #(requires 3.1.0 or greater)
include(CGAL_Eigen3_support)
if(NOT TARGET CGAL::Eigen3_support)
  message(
    STATUS
      "NOTICE: This project requires Eigen 3.1 (or greater) and will not be compiled."
  )
  return()
endif()

find_package(SCIP QUIET)
include(CGAL_SCIP_support)
if(NOT TARGET CGAL::SCIP_support)
  find_package(GLPK QUIET)
  include(CGAL_GLPK_support)
  if(NOT TARGET CGAL::GLPK_support)
    message(
      STATUS
        "NOTICE: This project requires either SCIP or GLPK, and will not be compiled."
    )
    return()
  endif()
endif()

create_single_source_cgal_program("polygonal_surface_reconstruction_test.cpp")
target_link_libraries(polygonal_surface_reconstruction_test
                      PUBLIC CGAL::Eigen3_support)
if(TARGET CGAL::SCIP_support)
  target_link_libraries(polygonal_surface_reconstruction_test
                        PUBLIC CGAL::SCIP_support)
else()
  target_link_libraries(polygonal_surface_reconstruction_test
                        PUBLIC CGAL::GLPK_support)
endif()
