cmake_minimum_required(VERSION 3.22)

project(NativeFileDialog VERSION 1.1.6 LANGUAGES NONE)
if (CMAKE_SYSTEM_NAME MATCHES "Windows")
	enable_language(CXX)
elseif (CMAKE_SYSTEM_NAME MATCHES "Darwin")
	enable_language(C)
endif()

add_library(NativeFileDialog STATIC
	"${CMAKE_CURRENT_SOURCE_DIR}/src/include/nfd.h"

	"${CMAKE_CURRENT_SOURCE_DIR}/src/common.h"
	"${CMAKE_CURRENT_SOURCE_DIR}/src/nfd_common.h"
	"${CMAKE_CURRENT_SOURCE_DIR}/src/simple_exec.h"

	"${CMAKE_CURRENT_SOURCE_DIR}/src/nfd_common.c"
	"$<$<PLATFORM_ID:Windows>:${CMAKE_CURRENT_SOURCE_DIR}/src/nfd_win.cpp>"
	"$<$<PLATFORM_ID:Darwin>:${CMAKE_CURRENT_SOURCE_DIR}/src/nfd_cocoa.m>"
)
add_library(NativeFileDialog::NativeFileDialog ALIAS NativeFileDialog)

target_include_directories(NativeFileDialog
	PUBLIC
		"${CMAKE_CURRENT_SOURCE_DIR}/src/include"
	PRIVATE
		"${CMAKE_CURRENT_SOURCE_DIR}/src"
)

target_link_libraries(NativeFileDialog
	PRIVATE
		"$<$<PLATFORM_ID:Windows>:ole32>"
		"$<$<PLATFORM_ID:Windows>:uuid>"
		"$<$<PLATFORM_ID:Darwin>:-framework Foundation>"
		"$<$<PLATFORM_ID:Darwin>:-framework AppKit>"
)

target_compile_definitions(NativeFileDialog
	PRIVATE
		"$<$<PLATFORM_ID:Windows>:_CRT_SECURE_NO_WARNINGS>"
		"$<$<PLATFORM_ID:Windows>:UNICODE>"
		"$<$<PLATFORM_ID:Windows>:_UNICODE>"
)
