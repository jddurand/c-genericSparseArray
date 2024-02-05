#
# Multi-config generator, e.g. Visual Studio on Windows:
#
# cmake -S c-genericSparseArray -B c-genericSparseArray-build
# cmake --build c-genericSparseArray-build --config RelWithDebInfo
# ctest --test-dir c-genericSparseArray-build -C RelWithDebInfo
# Windows:
#   cmake --install c-genericSparseArray-build --config RelWithDebInfo --prefix %cd%/c-genericSparseArray-install
# Others:
#   cmake --install c-genericSparseArray-build --config RelWithDebInfo --prefix `pwd`/c-genericSparseArray-install
# cmake --build c-genericSparseArray-build --config RelWithDebInfo --target package
#
# Single-config generator, e.g. NMake Makefiles on Windows, Unix Makefiles on Linxu:
#
# cmake -S c-genericSparseArray -B c-genericSparseArray-build -DCMAKE_BUILD_TYPE=RelWithDebInfo
# cmake --build c-genericSparseArray-build
# ctest --test-dir c-genericSparseArray-build
# Windows:
#   cmake --install c-genericSparseArray-build --prefix %cd%/c-genericSparseArray-install
# Others:
#   cmake --install c-genericSparseArray-build --prefix `pwd`/c-genericSparseArray-install
# cmake --build c-genericSparseArray-build --target package
#
cmake_minimum_required(VERSION 3.26.0 FATAL_ERROR)
project(genericSparseArray VERSION 1.0.55 LANGUAGES C)
#
# Get library helper
#
include(FetchContent)
if("x$ENV{CMAKE_HELPERS_DEPEND_CMAKE_HELPERS_FILE}" STREQUAL "x")
  FetchContent_Declare(cmake-helpers GIT_REPOSITORY https://github.com/jddurand/cmake-helpers.git GIT_SHALLOW TRUE)
else()
  FetchContent_Declare(cmake-helpers URL $ENV{CMAKE_HELPERS_DEPEND_CMAKE_HELPERS_FILE})
endif()
FetchContent_MakeAvailable(cmake-helpers)
#
# Dependencies
#
set(genericSparseArray_depends)
set(genericSparseArray_depends_ext)
set(genericSparseArray_find_dependencies)
#
# genericHash: public dependency
#
set(genericHash_git https://github.com/jddurand/c-genericHash.git)
cmake_helpers_depend(genericHash
  EXTERNALPROJECT_ADD_ARGS
    GIT_REPOSITORY ${genericHash_git}
    GIT_SHALLOW TRUE
  FIND_PACKAGE_ARGS
    REQUIRED
    CONFIG
)
list(APPEND genericSparseArray_find_dependencies "genericHash REQUIRED CONFIG")
list(APPEND genericSparseArray_depends PUBLIC genericHash::genericHash)
#
# Dependencies: test dependency
#
set(genericLogger_git https://github.com/jddurand/c-genericLogger.git)
cmake_helpers_depend(genericLogger
  EXTERNALPROJECT_ADD_ARGS
    GIT_REPOSITORY ${genericLogger_git}
    GIT_SHALLOW TRUE
  FIND_PACKAGE_ARGS
    REQUIRED
    CONFIG
  MAKEAVAILABLE FALSE
)
#
# Create library
#
cmake_helpers_library(genericSparseArray
  FIND_DEPENDENCIES ${genericSparseArray_find_dependencies}
  DEPENDS           ${genericSparseArray_depends}
  DEPENDS_EXT       ${genericSparseArray_depends_ext}
)
#
# Create test executables
#
include(CTest)
cmake_helpers_exe(genericSparseArrayTester
  TEST TRUE
  SOURCES test/genericSparseArray.c
  DEPENDS PUBLIC genericLogger::genericLogger)
#
# Package
#
cmake_helpers_package()
