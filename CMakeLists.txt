project(gnumake)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
	cmake_policy(SET CMP0005 NEW)
endif(COMMAND cmake_policy)

add_definitions(-D__APPLE__ -D__MACH__)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__ -U__linux__)
add_definitions(-D__ENVIRONMENT_MAC_OS_X_VERSION_MIN_REQUIRED__=1080)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w -ggdb -O0")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib${SUFFIX}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE) 
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

add_definitions(-DHAVE_CONFIG_H)
add_definitions(-DINCLUDEDIR=".")
add_definitions(-DLOCALEDIR="${CMAKE_INSTALL_PREFIX}/share/darling/gnumake")
add_definitions(-DLIBDIR="${CMAKE_INSTALL_PREFIX}/lib${SUFFIX}/darling")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(BEFORE ${CMAKE_CURRENT_SOURCE_DIR}/glob)

set(gnumake_SRCS
	alloca.c
	# ansi2knr.c
	ar.c
	arscan.c
	commands.c
	default.c
	dir.c
	expand.c
	file.c
	function.c
	getloadavg.c
	getopt1.c
	getopt.c
	hash.c
	implicit.c
	job.c
	main.c
	misc.c
	next.c
	read.c
	remake.c
	# remote-cstms.c
	remote-stub.c
	rule.c
	signame.c
	strcache.c
	variable.c
	version.c
	vpath.c
	glob/fnmatch.c
	glob/glob.c
)

include(darling_exe)
include(InstallSymlink)

add_darling_executable(gnumake ${gnumake_SRCS})
#target_link_libraries(bash ncurses)

install(TARGETS gnumake DESTINATION libexec/darling/usr/bin)

set(BINARY_PACKAGING_MODE ON)
InstallSymlink(gnumake libexec/darling/usr/bin/make)
