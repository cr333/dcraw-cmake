cmake_minimum_required(VERSION 2.8)
project(dcraw)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)

option(WITH_JPEG "Use libjpeg for decoding some compressed images and lossy Adobe DNGs." OFF)

add_executable(dcraw "dcraw.c")

if(WITH_JPEG)
	find_package(JPEG)
	include_directories(${JPEG_INCLUDE_DIR})
	target_link_libraries(dcraw ${JPEG_LIBRARIES})
else()
	add_definitions(/DNO_JPEG)
endif()

# TODO: Add support for JasPer and LCMS.
add_definitions(/DNO_JASPER)
add_definitions(/DNO_LCMS)

if(MSVC)
	# Disable some warnings.
	add_definitions(/D_CRT_SECURE_NO_WARNINGS)
	add_definitions(/wd4018) # '<' : signed/unsigned mismatch
	add_definitions(/wd4244) # '=' : conversion from 'double' to 'float', possible loss of data
	add_definitions(/wd4305) # '*=' : truncation from 'double' to 'float'
endif(MSVC)
