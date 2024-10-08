cmake_minimum_required(VERSION 3.19...3.30)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)
  message(FATAL_ERROR "Please specify build directory like:
  cmake -B build")
endif()

project(Fortran2018Examples
LANGUAGES Fortran
DESCRIPTION "Example of using modern Fortran syntax"
VERSION 1.4.0)

enable_testing()

include(CheckSourceCompiles)
include(CheckSourceRuns)

# --- compiler checks

set(f2003features f03abstract f03charalloc f03ieee f03utf8 f03selectType)
set(f2008features f08contig f08execute f08kind)
set(f2018features f18abstract f18assumed_rank f18prop f18random f2018rank)
set(f2023features f2023rank_integer f2023ternary f2023tokenize f2023real16)

foreach(i IN LISTS f2003features f2008features f2018features f2023features)
  include(cmake/${i}.cmake)
endforeach()

include(cmake/compilers.cmake)

set(CMAKE_Fortran_MODULE_DIRECTORY ${PROJECT_BINARY_DIR}/include)
# for ease of use

foreach(a IN ITEMS array character compile debug io maximum namelist
         overloading pointer real standard submodule)
  add_subdirectory(test/${a})
endforeach()

foreach(a IN ITEMS git system)
  add_subdirectory(src/${a})
endforeach()

file(GENERATE OUTPUT .gitignore CONTENT "*")

include(cmake/features.cmake)
