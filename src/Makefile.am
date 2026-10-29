AUTOMAKE_OPTIONS = subdir-objects

AM_CFLAGS = $(VCACHEAPI_CFLAGS) @OSSP_CFLAGS@ -DSTATIC_ASSERT -Wall -Werror -Wextra -std=c99
AM_LDFLAGS = $(VCACHEAPI_LIBS) $(VMOD_LDFLAGS) @OSSP_LIBS@

vmod_LTLIBRARIES = libvmod_uuid.la

libvmod_uuid_la_SOURCES = \
	vmod_uuid.c

nodist_libvmod_uuid_la_SOURCES = \
	vcc_if.c \
	vcc_if.h

DISTCLEANFILES = vmod_vcs_version.txt

vmod_vcs_version.txt: $(nodist_libvmod_frozen_la_SOURCES)

vmod_uuid.lo: vcc_if.c vcc_if.h

vmod_uuid.lo: $(nodist_libvmod_uuid_la_SOURCES)

vcc_if.c: vcc_if.h

vcc_if.h: vmod_uuid.vcc
	$(AM_V_VMODTOOL) $(PYTHON) $(VMODTOOL) -o vcc_if $(srcdir)/vmod_uuid.vcc

AM_TESTS_ENVIRONMENT = \
	PATH="$(VMOD_TEST_PATH)"
TEST_EXTENSIONS = .vtc
VTC_LOG_COMPILER = vtest -E@VTESTEXT@ -v
AM_VTC_LOG_FLAGS = -Dvmod_uuid="$(VMOD_UUID)"

TESTS = @VMOD_TESTS@

EXTRA_DIST = \
	vmod_uuid.vcc \
	$(VMOD_TESTS)

CLEANFILES = \
	$(builddir)/vcc_if.c \
	$(builddir)/vcc_if.h \
	$(builddir)/vmod_uuid.rst \
	$(builddir)/vmod_uuid.man.rst
