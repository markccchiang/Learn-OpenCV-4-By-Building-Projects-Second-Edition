cmake_minimum_required (VERSION 3.0) 

PROJECT(Segment_text_ch11)

set (CMAKE_CXX_STANDARD 11)

IF(EXISTS ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    conan_basic_setup()
ENDIF()

# Requires OpenCV v4.0 or later
FIND_PACKAGE( OpenCV 4 REQUIRED )

include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})

FIND_PACKAGE(Protobuf REQUIRED)

find_package( PkgConfig REQUIRED)

pkg_search_module( TESSERACT REQUIRED tesseract )

pkg_search_module( LEPTONICA REQUIRED lept )

include_directories( ${TESSERACT_INCLUDE_DIRS} )

include_directories( ${LEPTONICA_INCLUDE_DIRS} )

link_directories( ${TESSERACT_LIBRARY_DIRS} )

link_directories( ${LEPTONICA_LIBRARY_DIRS} )

ADD_EXECUTABLE( detection detection.cpp )
TARGET_LINK_LIBRARIES( detection  ${CONAN_LIBS} ${OpenCV_LIBS}  -pthread ${PROTOBUF_LIBRARY} -lpthread ${TESSERACT_LIBRARIES} ${LEPTONICA_LIBRARIES} )

ADD_EXECUTABLE( ocr ocr.cpp )
TARGET_LINK_LIBRARIES( ocr  ${CONAN_LIBS} ${OpenCV_LIBS}  -pthread ${PROTOBUF_LIBRARY} ${TESSERACT_LIBRARIES} ${LEPTONICA_LIBRARIES})

