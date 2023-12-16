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
project(genericSparseArray VERSION 1.0.53 LANGUAGES C)
#
# Get library helper
#
include(FetchContent)
FetchContent_Declare(cmake-helpers GIT_REPOSITORY https://github.com/jddurand/cmake-helpers.git GIT_SHALLOW TRUE)
FetchContent_MakeAvailable(cmake-helpers)
#
# Dependencies
#
cmake_helpers_depend(genericHash
  EXTERNALPROJECT_ADD_ARGS GIT_REPOSITORY https://github.com/jddurand/c-genericHash.git GIT_SHALLOW TRUE
  FIND_PACKAGE_ARGS REQUIRED CONFIG
)
#
# Dependencies
#
cmake_helpers_depend(genericLogger
  EXTERNALPROJECT_ADD_ARGS GIT_REPOSITORY https://github.com/jddurand/c-genericLogger.git GIT_SHALLOW TRUE
  FIND_PACKAGE_ARGS REQUIRED CONFIG
)
#
# Create library
#
cmake_helpers_library(genericSparseArray
  FIND_DEPENDENCIES "genericHash REQUIRED CONFIG"
  DEPENDS PUBLIC genericHash::genericHash
)
#
# Create test executables
#
cmake_helpers_exe(genericSparseArrayTester TEST TRUE SOURCES test/genericSparseArray.c DEPENDS PUBLIC genericLogger::genericLogger)
#
# Package
#
cmake_helpers_package()
