CMAKE_MINIMUM_REQUIRED (VERSION 3.0.0 FATAL_ERROR)
INCLUDE ("3rdparty/github/cmake-utils-remote/cmake/MyPackageStart.cmake")
MYPACKAGESTART (genericSparseArray 1 0 8)

#############################
# Dependencies
#############################
INCLUDE (MyPackageDependency)

# ----------------------
# genericStack: required
# ----------------------
MYPACKAGEDEPENDENCY(genericStack "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/github/genericStack-remote" GENERICSTACK_REAL_INCLUDE_DIRS)

# ----------------------
# genericHash: required
# ----------------------
MYPACKAGEDEPENDENCY(genericHash "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/github/genericHash-remote" GENERICHASH_REAL_INCLUDE_DIRS)

# -----------------------------------
# genericLogger: required (for tests)
# -----------------------------------
MYPACKAGEDEPENDENCY(genericLogger "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/github/genericLogger-remote" GENERICLOGGER_REAL_INCLUDE_DIRS)

INCLUDE_DIRECTORIES (
  "${PROJECT_SOURCE_DIR}/include"                      # Include path for the other headers
  ${GENERICLOGGER_REAL_INCLUDE_DIRS}                   # Include path for genericLogger (external - required)
  ${GENERICSTACK_REAL_INCLUDE_DIRS}                    # Include path for genericStack (external - required)
  ${GENERICHASH_REAL_INCLUDE_DIRS}                     # Include path for genericHash (external - required)
  )

# -----
# Check
# -----
ENABLE_TESTING()

ADD_EXECUTABLE        (genericSparseArrayTester       EXCLUDE_FROM_ALL test/genericSparseArray.c)
ADD_EXECUTABLE        (genericSparseArrayTesterStatic EXCLUDE_FROM_ALL test/genericSparseArray.c)

IF (ALL_IN_ONE)
  TARGET_LINK_LIBRARIES (genericSparseArrayTester       genericLogger)
  TARGET_LINK_LIBRARIES (genericSparseArrayTesterStatic genericLogger_static)
ELSE ()
  TARGET_LINK_LIBRARIES (genericSparseArrayTester       ${GENERICLOGGER_LIBRARIES})
  TARGET_LINK_LIBRARIES (genericSparseArrayTesterStatic ${GENERICLOGGER_STATIC_LIBRARIES})
ENDIF ()

SET_TARGET_PROPERTIES (genericSparseArrayTester       PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${LIBRARY_OUTPUT_PATH})
SET_TARGET_PROPERTIES (genericSparseArrayTesterStatic PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${LIBRARY_OUTPUT_PATH})

INCLUDE (MyPackageCheck)
MYPACKAGECHECK (genericSparseArrayTest       "${LIBRARY_OUTPUT_PATH}/genericSparseArrayTester")
MYPACKAGECHECK (genericSparseArrayTestStatic "${LIBRARY_OUTPUT_PATH}/genericSparseArrayTester")

# -------
# Install
# -------
INSTALL (FILES include/genericSparseArray.h DESTINATION include)

INCLUDE (MyPackageMan)
MYPACKAGEMAN("${PROJECT_SOURCE_DIR}/README.pod" "GENERICSPARSEARRAY" "3" "${GENERICSPARSEARRAY_VERSION}")

#############
# Packaging #
#############
SET (CPACK_PACKAGE_VERSION_MAJOR "${GENERICSPARSEARRAY_VERSION_MAJOR}")
SET (CPACK_PACKAGE_VERSION_MINOR "${GENERICSPARSEARRAY_VERSION_MINOR}")
SET (CPACK_PACKAGE_VERSION_PATCH "${GENERICSPARSEARRAY_VERSION_PATCH}")
CONFIGURE_FILE ("${PROJECT_SOURCE_DIR}/LICENSE"  "${CMAKE_CURRENT_BINARY_DIR}/LICENSE.txt")
SET (CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_BINARY_DIR}/LICENSE.txt")
INCLUDE (CPack)
