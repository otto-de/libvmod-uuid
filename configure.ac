AC_PREREQ(2.68)
AC_COPYRIGHT([Copyright (c) 2013 Sharecare, Inc.])
AC_INIT([libvmod-uuid], [trunk], [], [vmod-uuid])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR(src/vmod_uuid.vcc)
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_HEADERS(config.h)

AC_CANONICAL_SYSTEM
AC_LANG(C)

AM_INIT_AUTOMAKE([1.12 -Wall -Werror foreign parallel-tests])
AM_SILENT_RULES([yes])
AM_PROG_AR

AC_CHECK_LIB(ossp-uuid, uuid_create)

LT_PREREQ([2.2.6])
LT_INIT([dlopen disable-static])

AX_PTHREAD(,[AC_MSG_ERROR([Could not configure pthreads support])])

LIBS="$PTHREAD_LIBS $LIBS"
CFLAGS="$CFLAGS $PTHREAD_CFLAGS"
CC="$PTHREAD_CC"

AC_ARG_WITH([rst2man],
	AS_HELP_STRING(
		[--with-rst2man=PATH],
		[Location of rst2man (auto)]),
	[RST2MAN="$withval"],
	[AC_CHECK_PROGS(RST2MAN, [rst2man rst2man.py], [])])

m4_ifndef([VARNISH_PREREQ], AC_MSG_ERROR([Need varnish.m4 -- see INSTALL.rst]))

VARNISH_PREREQ([5.0.0])
VARNISH_VMODS([uuid])

VMOD_TESTS="$(cd $srcdir/src && echo tests/*.vtc)"
AC_SUBST(VMOD_TESTS)

PKG_CHECK_VAR([LIBVARNISHAPI_LIBDIR], [varnishapi], [libdir])
AC_SUBST([VARNISH_LIBRARY_PATH],
	[$LIBVARNISHAPI_LIBDIR:$LIBVARNISHAPI_LIBDIR/varnish])

AC_CONFIG_FILES([
	Makefile
	src/Makefile
])

AM_CONDITIONAL(HAVE_RST2MAN, [test "x$RST2MAN" != "xno"])

# Checks for C sources
AC_HEADER_STDC
AC_C_INLINE
AC_TYPE_SIZE_T
AC_CHECK_HEADERS([string.h])
AC_CHECK_HEADERS([uuid.h])
AC_CHECK_HEADERS([stdarg.h])

PKG_CHECK_MODULES([OSSP], [ossp-uuid])

# --enable-stack-protector
AC_ARG_ENABLE(stack-protector,
       AS_HELP_STRING([--enable-stack-protector],[enable stack protector (default is YES)]),
       [],
       [enable_stack_protector=yes])

if test "x$enable_stack_protector" != "xno"; then
	AX_CHECK_COMPILE_FLAG([-fstack-protector],
		AX_CHECK_LINK_FLAG([-fstack-protector],
			[CFLAGS="${CFLAGS} -fstack-protector"], [], []),
		[], [])
fi

# --enable-debugging
AC_ARG_ENABLE(debugging,
	AS_HELP_STRING([--enable-debugging],[enable debugging (default is NO)]),
	[],
	[enable_debugging=no])

# AC_PROG_CC sets CFLAGS to '-g -O2' unless already set, so there's no
# need to add -g. Disable or change by explicitly setting CFLAGS. If
# this option is enabled, then -Og or -O0 becomes the last
# optimization option, and hence takes precedence.
if test "x$enable_debugging" != "xno"; then
	CFLAGS="${CFLAGS} -fno-inline"
	AX_CHECK_COMPILE_FLAG([-Og],
		[CFLAGS="${CFLAGS} -Og"],
		[CFLAGS="${CFLAGS} -O0"],
		[])
fi

AC_OUTPUT
