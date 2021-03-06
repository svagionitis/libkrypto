project(LIBKRYPTO C)

cmake_minimum_required(VERSION 2.6)

set(CMAKE_COLOR_MAKEFILE ON)
set(CMAKE_VERBOSE_MAKEFILE ON)

set(CMAKE_BUILD_TYPE Debug)

if (CMAKE_BUILD_TYPE MATCHES "Debug")
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DDEBUG")
endif (CMAKE_BUILD_TYPE MATCHES "Debug")

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})


# If it's linux, add POSIX standards definition of some extra functions.
# For more info see http://stackoverflow.com/questions/5378778/what-does-d-xopen-source-do-mean
if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    set(CMAKE_C_FLAGS "-D_BSD_SOURCE -std=c99 -Wall -W -O2 -D_XOPEN_SOURCE=700")
endif()

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(CMAKE_C_FLAGS "-D_BSD_SOURCE -std=c99 -Wall -W -O2")
endif()


if (DEFINED LIBKRYPTO_SHARED)
    option(LIBKRYPTO_SHARED "Build shared lib" ${LIBKRYPTO_SHARED})
else()
    option(LIBKRYPTO_SHARED "Build shared lib" ON)
endif()

if (DEFINED LIBKRYPTO_STATIC)
    option(LIBKRYPTO_STATIC "Build static lib" ${LIBKRYPTO_STATIC})
else()
    option(LIBKRYPTO_STATIC "Build static lib" OFF)
endif()

# Check header files
include(CheckIncludeFiles)

# Check if sys/select.h exists
check_include_files("sys/select.h" HAVE_SYS_SELECT_H)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/select/select.h")
    if (HAVE_SYS_SELECT_H)
	message("Use of ${CMAKE_SOURCE_DIR}/src/select/select.h2")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/select/select.h2"
			"${CMAKE_SOURCE_DIR}/src/select/select.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/select/select.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/select/select.h2")
	endif()
    else()
	message("Use of ${CMAKE_SOURCE_DIR}/src/select/select.h1")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/select/select.h1"
			"${CMAKE_SOURCE_DIR}/src/select/select.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/select/select.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/select/select.h1")
	endif()
    endif()
endif()

# Check if poll.h exists
check_include_files("poll.h" HAVE_POLL_H)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h")
    if (HAVE_POLL_H)
	message("Use of ${CMAKE_SOURCE_DIR}/src/iopause/iopause.h2")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h2"
			"${CMAKE_SOURCE_DIR}/src/iopause/iopause.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h2")
	endif()
    else()
	message("Use of ${CMAKE_SOURCE_DIR}/src/iopause/iopause.h1")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h1"
			"${CMAKE_SOURCE_DIR}/src/iopause/iopause.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h1")
	endif()
    endif()
endif()

# Check if dirent.h exists
check_include_files("dirent.h" HAVE_DIRENT_H)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h")
    if (HAVE_DIRENT_H)
        message("Use of ${CMAKE_SOURCE_DIR}/src/direntry/direntry.h2")
        if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
            file(RENAME "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h2"
                        "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h")
        else()
            file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h"
                          INPUT "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h2")
        endif()
    else()
        message("Use of ${CMAKE_SOURCE_DIR}/src/dirent/direntry.h1")
        if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
            file(RENAME "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h1"
                        "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h")
        else()
            file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h"
                          INPUT "${CMAKE_SOURCE_DIR}/src/direntry/direntry.h1")
        endif()
    endif()
endif()

# Check if sys/wait.h exists
check_include_files("sys/wait.h" HAVE_SYS_WAIT_H)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h")
    if (HAVE_SYS_WAIT_H)
	message("Use of ${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h2")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h2"
			"${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h2")
	endif()
    else()
	message("Use of ${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h1")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h1"
			"${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/haswaitp/haswaitp.h1")
	endif()
    endif()
endif()


# Check type sizes
include(CheckTypeSize)

# Check unsigned long size
check_type_size("unsigned long" ULONG_SIZE)
message("Size of unsigned long is ${ULONG_SIZE}")
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h")
    if (ULONG_SIZE MATCHES "8")
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h2"
			"${CMAKE_SOURCE_DIR}/src/uint32/uint32.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h2")
	endif()
    else()
       if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h1"
			"${CMAKE_SOURCE_DIR}/src/uint32/uint32.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h1")
	endif()
    endif()
endif()

# Checks whether the given function exists.
include(CheckFunctionExists)

# Check if vfork exists
check_function_exists(vfork HAVE_VFORK)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/fork/fork.h")
    if (HAVE_VFORK)
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/fork/fork.h2"
			"${CMAKE_SOURCE_DIR}/src/fork/fork.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/fork/fork.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/fork/fork.h2")
	endif()
    else()
       if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/fork/fork.h1"
			"${CMAKE_SOURCE_DIR}/src/fork/fork.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/fork/fork.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/fork/fork.h1")
	endif()
    endif()
endif()

# Check if sigprocmask exists
check_function_exists(sigprocmask HAVE_SGPRM)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h")
    if (HAVE_SGPRM)
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h2"
			"${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h2")
	endif()
    else()
       if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h1"
			"${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/hassgprm/hassgprm.h1")
	endif()
    endif()
endif()

# Check if sigaction exists
check_function_exists(sigaction HAVE_SGACT)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h")
    if (HAVE_SGACT)
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h2"
			"${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h2")
	endif()
    else()
       if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h1"
			"${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/hassgact/hassgact.h1")
	endif()
    endif()
endif()

# Check if mkfifo exists
check_function_exists(mkfifo HAVE_MKFFO)
if (NOT EXISTS "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h")
    if (HAVE_MKFFO)
	if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h2"
			"${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h2")
	endif()
    else()
       if (${CMAKE_VERSION} VERSION_LESS "3.0.0")
	    file(RENAME "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h1"
			"${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h")
	else()
	    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h"
			  INPUT "${CMAKE_SOURCE_DIR}/src/hasmkffo/hasmkffo.h1")
	endif()
    endif()
endif()


include_directories(src/alloc)
include_directories(src/buffer)
include_directories(src/byte)
include_directories(src/case)
include_directories(src/coe)
include_directories(src/datetime)
include_directories(src/deepsleep)
include_directories(src/env)
include_directories(src/error)
include_directories(src/exit)
include_directories(src/fd)
include_directories(src/fifo)
include_directories(src/fmt)
include_directories(src/fork)
include_directories(src/getln)
include_directories(src/hasmkffo)
include_directories(src/hassgact)
include_directories(src/hassgprm)
include_directories(src/haswaitp)
include_directories(src/iopause)
include_directories(src/match)
include_directories(src/open)
include_directories(src/pathexec)
include_directories(src/scan)
include_directories(src/seek)
include_directories(src/select)
include_directories(src/sig)
include_directories(src/socket)
include_directories(src/str)
include_directories(src/stralloc)
include_directories(src/strerr)
include_directories(src/subfd)
include_directories(src/substdio)
include_directories(src/tai)
include_directories(src/taia)
include_directories(src/uint16)
include_directories(src/uint32)
include_directories(src/uint64)
include_directories(src/wait)

set(alloc_SRCS
    src/alloc/alloc.h
    src/alloc/alloc.c
    src/alloc/alloc_re.c
    )

set(buffer_SRCS
    src/buffer/buffer.h
    src/buffer/buffer.c
    src/buffer/buffer_0.c
    src/buffer/buffer_1.c
    src/buffer/buffer_1s.c
    src/buffer/buffer_2.c
    src/buffer/buffer_copy.c
    src/buffer/buffer_get.c
    src/buffer/buffer_put.c
    src/buffer/buffer_read.c
    src/buffer/buffer_write.c
    )

set(byte_SRCS
    src/byte/byte.h
    src/byte/byte_chr.c
    src/byte/byte_copy.c
    src/byte/byte_cr.c
    src/byte/byte_diff.c
    src/byte/byte_rchr.c
    src/byte/byte_zero.c
    )

set(case_SRCS
    src/case/case.h
    src/case/case_diffb.c
    src/case/case_diffs.c
    src/case/case_lowerb.c
    src/case/case_lowers.c
    src/case/case_starts.c
    )

set(coe_SRCS
    src/coe/coe.h
    src/coe/coe.c
    )

set(datetime_SRCS
    src/datetime/datetime.h
    src/datetime/datetime.c
    src/datetime/datetime_un.c
    src/datetime/date822fmt.h
    src/datetime/date822fmt.c
    )

set(deepsleep_SRCS
    src/deepsleep/deepsleep.h
    src/deepsleep/deepsleep.c
    )

set(env_SRCS
    src/env/env.h
    src/env/env.c
    src/env/envread.c
    )

set(error_SRCS
    src/error/error.h
    src/error/error.c
    src/error/error_str.c
    src/error/error_temp.c
    )

set(fd_SRCS
    src/fd/fd.h
    src/fd/fd_copy.c
    src/fd/fd_move.c
    )

set(fifo_SRCS
    src/fifo/fifo.h
    src/fifo/fifo.c
    )

set(fmt_SRCS
    src/fmt/auto_split.h
    src/fmt/fmt.h
    src/fmt/fmt_str.c
    src/fmt/fmt_strn.c
    src/fmt/fmt_uint.c
    src/fmt/fmt_uint0.c
    src/fmt/fmt_ulong.c
    src/fmt/fmtqfn.h
    src/fmt/fmtqfn.c
    )

set(getln_SRCS
    src/getln/getln.h
    src/getln/getln.c
    src/getln/getln2.c
    )

set(iopause_SRCS
    src/iopause/iopause.h
    src/iopause/iopause.c
    )

set(match_SRCS
    src/match/match.h
    src/match/match.c
    )

set(open_SRCS
    src/open/open.h
    src/open/readwrite.h
    src/open/open_read.c
    src/open/open_trunc.c
    src/open/readclose.h
    src/open/readclose.c
    src/open/openreadclose.h
    src/open/openreadclose.c
    )

set(pathexec_SRCS
    src/pathexec/pathexec.h
    src/pathexec/pathexec_env.c
    src/pathexec/pathexec_run.c
    )

set(scan_SRCS
    src/scan/scan.h
    src/scan/scan_ulong.c
    )

set(seek_SRCS
    src/seek/seek.h
    src/seek/seek_set.c
    )

set(sig_SRCS
    src/sig/sig.h
    src/sig/sig.c
    src/sig/sig_block.c
    src/sig/sig_catch.c
    src/sig/sig_pause.c
    )

set(socket_SRCS
    src/socket/ndelay.h
    src/socket/ndelay_off.c
    src/socket/ndelay_on.c
    src/socket/socket.h
    src/socket/socket_accept.c
    src/socket/socket_bind.c
    src/socket/socket_conn.c
    src/socket/socket_listen.c
    src/socket/socket_recv.c
    src/socket/socket_send.c
    src/socket/socket_tcp.c
    src/socket/socket_udp.c
    )

set(str_SRCS
    src/str/str.h
    src/str/str_chr.c
    src/str/str_cpy.c
    src/str/str_diff.c
    src/str/str_diffn.c
    src/str/str_len.c
    src/str/str_rchr.c
    src/str/str_start.c
    )

set(stralloc_SRCS
    src/stralloc/gen_alloc.h
    src/stralloc/gen_allocdefs.h
    src/stralloc/stralloc.h
    src/stralloc/stralloc_cat.c
    src/stralloc/stralloc_catb.c
    src/stralloc/stralloc_cats.c
    src/stralloc/stralloc_copy.c
    src/stralloc/stralloc_eady.c
    src/stralloc/stralloc_num.c
    src/stralloc/stralloc_opyb.c
    src/stralloc/stralloc_opys.c
    src/stralloc/stralloc_pend.c
    )

set(strerr_SRCS
    src/strerr/strerr.h
    src/strerr/strerr_die.c
    src/strerr/strerr_sys.c
    )

set(subfd_SRCS
    src/subfd/subfd.h
    src/subfd/subfderr.c
    src/subfd/subfdin.c
    src/subfd/subfdins.c
    src/subfd/subfdout.c
    src/subfd/subfdouts.c
    )

set(substdio_SRCS
    src/substdio/substdio.h
    src/substdio/substdio.c
    src/substdio/substdio_copy.c
    )

set(tai_SRCS
    src/tai/tai.h
    src/tai/tai_add.c
    src/tai/tai_now.c
    src/tai/tai_pack.c
    src/tai/tai_sub.c
    src/tai/tai_uint.c
    src/tai/tai_unpack.c
    )

set(taia_SRCS
    src/taia/taia.h
    src/taia/taia_add.c
    src/taia/taia_approx.c
    src/taia/taia_frac.c
    src/taia/taia_less.c
    src/taia/taia_now.c
    src/taia/taia_pack.c
    src/taia/taia_sub.c
    src/taia/taia_tai.c
    src/taia/taia_uint.c
    )

set(uint16_SRCS
    src/uint16/uint16.h
    src/uint16/uint16_pack.c
    src/uint16/uint16_unpack.c
    )

set(uint32_SRCS
    src/uint32/uint32.h
    src/uint32/uint32_pack.c
    src/uint32/uint32_unpack.c
    )

set(wait_SRCS
    src/wait/wait.h
    src/wait/wait_nohang.c
    src/wait/wait_pid.c
    )

set(LIB_TARGET kr1pt0)
set(LIB_SRC
    ${alloc_SRCS}
    ${buffer_SRCS}
    ${byte_SRCS}
    ${case_SRCS}
    ${coe_SRCS}
    ${datetime_SRCS}
    ${deepsleep_SRCS}
    ${env_SRCS}
    ${error_SRCS}
    ${fd_SRCS}
    ${fifo_SRCS}
    ${fmt_SRCS}
    ${getln_SRCS}
    ${iopause_SRCS}
    ${match_SRCS}
    ${open_SRCS}
    ${pathexec_SRCS}
    ${scan_SRCS}
    ${seek_SRCS}
    ${sig_SRCS}
    ${socket_SRCS}
    ${str_SRCS}
    ${stralloc_SRCS}
    ${strerr_SRCS}
    ${subfd_SRCS}
    ${substdio_SRCS}
    ${tai_SRCS}
    ${taia_SRCS}
    ${uint16_SRCS}
    ${uint32_SRCS}
    ${wait_SRCS}
    )


# Shared library
if (LIBKRYPTO_SHARED)
    if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
        set(CMAKE_SHARED_LIBRARY_SUFFIX ".so")
        set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -export-dynamic")
    endif()
    if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
        set(CMAKE_SHARED_LIBRARY_SUFFIX ".dylib")
        set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -flat_namespace -single_module -undefined dynamic_lookup")
    endif()
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/lib")
    add_library(${LIB_TARGET} SHARED ${LIB_SRC})
endif()

#Static library
if (LIBKRYPTO_STATIC)
    set(LIB_STATIC_TARGET "${LIB_TARGET}-static")
    set(CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_STATIC_LIBRARY_SUFFIX})
    set(CMAKE_STATIC_LIBRARY_SUFFIX ".a")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/lib")
    add_library(${LIB_STATIC_TARGET} STATIC ${LIB_SRC})
    set_target_properties(${LIB_STATIC_TARGET} PROPERTIES OUTPUT_NAME ${LIB_TARGET})
endif()

link_directories(${PROJECT_SOURCE_DIR}/lib)

