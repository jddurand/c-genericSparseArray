#
# Multi-config generator, e.g. Visual Studio on Windows:
#
# cmake -S c-genericSpaceArray -B c-genericSpaceArray-build
# cmake --build c-genericSpaceArray-build --config RelWithDebInfo
# ctest --test-dir c-genericSpaceArray-build -C RelWithDebInfo
# Windows:
#   cmake --install c-genericSpaceArray-build --config RelWithDebInfo --prefix %cd%/c-genericSpaceArray-install
# Others:
#   cmake --install c-genericSpaceArray-build --config RelWithDebInfo --prefix `pwd`/c-genericSpaceArray-install
# cmake --build c-genericSpaceArray-build --config RelWithDebInfo --target package
#
# Single-config generator, e.g. NMake Makefiles on Windows, Unix Makefiles on Linxu:
#
# cmake -S c-genericSpaceArray -B c-genericSpaceArray-build -DCMAKE_BUILD_TYPE=RelWithDebInfo
# cmake --build c-genericSpaceArray-build
# ctest --test-dir c-genericSpaceArray-build
# Windows:
#   cmake --install c-genericSpaceArray-build --prefix %cd%/c-genericSpaceArray-install
# Others:
#   cmake --install c-genericSpaceArray-build --prefix `pwd`/c-genericSpaceArray-install
# cmake --build c-genericSpaceArray-build --target package
#
cmake_minimum_required(VERSION 3.26.0 FATAL_ERROR)
project(genericSpaceArray VERSION 1.0.53 LANGUAGES C)
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
