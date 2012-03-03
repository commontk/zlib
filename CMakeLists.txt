cmake_minimum_required(VERSION 2.8)

PROJECT(ZLIB)

INCLUDE_DIRECTORIES(
  "${ZLIB_SOURCE_DIR}"
  "${ZLIB_SOURCE_DIR}"
  "${ZLIB_BINARY_DIR}"
  )

# source files for zlib
SET(ZLIB_SRCS
  adler32.c  compress.c  crc32.c  deflate.c  gzio.c   inffast.c
  inflate.c  inftrees.c  trees.c  uncompr.c  zutil.c
  )

# for windows add the .def and .rc files to the source list
# if building shared libs
IF(WIN32)
  IF(BUILD_SHARED_LIBS)
    SET(ZLIB_DLL 1)
    IF(NOT UNIX)
      IF(NOT BORLAND)
        IF(NOT MINGW)
          SET(ZLIB_SRCS ${ZLIB_SRCS} zlib.def zlib.rc  )
        ENDIF(NOT MINGW)
      ENDIF(NOT BORLAND) 
    ENDIF(NOT UNIX)   
  ENDIF(BUILD_SHARED_LIBS)
ENDIF(WIN32)

CONFIGURE_FILE(${ZLIB_SOURCE_DIR}/.NoDartCoverage
  ${ZLIB_BINARY_DIR}/.NoDartCoverage)
CONFIGURE_FILE(${ZLIB_SOURCE_DIR}/zlibDllConfig.h.in
  ${ZLIB_BINARY_DIR}/zlibDllConfig.h)

FOREACH(name zlib zconf cm_zlib_mangle)
  CONFIGURE_FILE(${ZLIB_SOURCE_DIR}/${name}.h
    ${ZLIB_BINARY_DIR}/${name}.h COPYONLY)
ENDFOREACH(name)


ADD_LIBRARY(zlib ${ZLIB_SRCS})

#INSTALL(FILES Copyright.txt DESTINATION ${CMake_DOC_DEST}/cmzlib)
