AC_PREREQ([2.68])
AC_INIT(firejail, 0.9.53, netblue30@yahoo.com, , http://firejail.wordpress.com)
AC_CONFIG_SRCDIR([src/firejail/main.c])
#AC_CONFIG_HEADERS([config.h])

AC_PROG_CC
#AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_RANLIB

HAVE_SPECTRE="no"
AC_MSG_CHECKING(for Spectre mitigation support in gcc or clang compiler)
AS_IF([test "$CC" = "gcc"], [
	HAVE_SPECTRE="yes"
	$CC -mindirect-branch=thunk -c dummy.c || HAVE_SPECTRE="no"
	rm -f dummy.o
	AS_IF([test "$HAVE_SPECTRE" = "yes"], [
		EXTRA_CFLAGS+=" -mindirect-branch=thunk "
	])
])
AS_IF([test "$CC" = "clang"], [
	HAVE_SPECTRE="yes"
	$CC -mretpoline -c dummy.c || HAVE_SPECTRE="no"
	rm -f dummy.o
	AS_IF([test "$HAVE_SPECTRE" = "yes"], [
		EXTRA_CFLAGS+=" -mretpoline "
	])
])
AS_IF([test "$HAVE_SPECTRE" = "yes"], [
	AC_MSG_RESULT(yes)
])
AS_IF([test "$HAVE_SPECTRE" = "no"], [
	AC_MSG_RESULT(... not available)
])
AC_SUBST([EXTRA_CFLAGS])

HAVE_APPARMOR=""
AC_ARG_ENABLE([apparmor],
    AS_HELP_STRING([--enable-apparmor], [enable apparmor]))
AS_IF([test "x$enable_apparmor" = "xyes"], [
	HAVE_APPARMOR="-DHAVE_APPARMOR"
	AC_SUBST(HAVE_APPARMOR)
])

AS_IF([test "x$enable_apparmor" = "xyes"], [
	AC_CHECK_HEADER(sys/apparmor.h, , [AC_MSG_ERROR(
	[Couldn't find sys/apparmor.h... please install apparmor user space library and development files] )])
])
AS_IF([test "x$enable_apparmor" = "xyes"], [
	EXTRA_LDFLAGS+=" -lapparmor "
])
AC_SUBST([EXTRA_LDFLAGS])

HAVE_OVERLAYFS=""
AC_ARG_ENABLE([overlayfs],
    AS_HELP_STRING([--disable-overlayfs], [disable overlayfs]))
AS_IF([test "x$enable_overlayfs" != "xno"], [
	HAVE_OVERLAYFS="-DHAVE_OVERLAYFS"
	AC_SUBST(HAVE_OVERLAYFS)
])

HAVE_PRIVATEHOME=""
AC_ARG_ENABLE([private-home],
    AS_HELP_STRING([--disable-private-home], [disable private home feature]))
AS_IF([test "x$enable_private_home" != "xno"], [
	HAVE_PRIVATE_HOME="-DHAVE_PRIVATE_HOME"
	AC_SUBST(HAVE_PRIVATE_HOME)
])

HAVE_SECCOMP=""
AC_ARG_ENABLE([seccomp],
    AS_HELP_STRING([--disable-seccomp], [disable seccomp]))
AS_IF([test "x$enable_seccomp" != "xno"], [
	HAVE_SECCOMP="-DHAVE_SECCOMP"
	AC_SUBST(HAVE_SECCOMP)
])

HAVE_CHROOT=""
AC_ARG_ENABLE([chroot],
    AS_HELP_STRING([--disable-chroot], [disable chroot]))
AS_IF([test "x$enable_chroot" != "xno"], [
	HAVE_CHROOT="-DHAVE_CHROOT"
	AC_SUBST(HAVE_CHROOT)
])

HAVE_BIND=""
AC_ARG_ENABLE([bind],
    AS_HELP_STRING([--disable-bind], [disable bind]))
AS_IF([test "x$enable_bind" != "xno"], [
	HAVE_BIND="-DHAVE_BIND"
	AC_SUBST(HAVE_BIND)
])

HAVE_GLOBALCFG=""
AC_ARG_ENABLE([globalcfg],
    AS_HELP_STRING([--disable-globalcfg], [if the global config file firejail.cfg is not present, continue the program using defaults]))
AS_IF([test "x$enable_globalcfg" != "xno"], [
	HAVE_GLOBALCFG="-DHAVE_GLOBALCFG"
	AC_SUBST(HAVE_GLOBALCFG)
])

HAVE_NETWORK=""
AC_ARG_ENABLE([network],
    AS_HELP_STRING([--disable-network], [disable network]))
AC_ARG_ENABLE([network],
    AS_HELP_STRING([--enable-network=restricted], [ restrict --net= to root only]))
AS_IF([test "x$enable_network" != "xno"], [
	HAVE_NETWORK="-DHAVE_NETWORK"
	AS_IF([test "x$enable_network" = "xrestricted"], [
	       HAVE_NETWORK="$HAVE_NETWORK -DHAVE_NETWORK_RESTRICTED"
	])
	AC_SUBST(HAVE_NETWORK)
])

HAVE_USERNS=""
AC_ARG_ENABLE([userns],
    AS_HELP_STRING([--disable-userns], [disable user namespace]))
AS_IF([test "x$enable_userns" != "xno"], [
	HAVE_USERNS="-DHAVE_USERNS"
	AC_SUBST(HAVE_USERNS)
])

HAVE_X11=""
AC_ARG_ENABLE([x11],
    AS_HELP_STRING([--disable-x11], [disable X11 sandboxing support]))
AS_IF([test "x$enable_x11" != "xno"], [
	HAVE_X11="-DHAVE_X11"
	AC_SUBST(HAVE_X11)
])

HAVE_FILE_TRANSFER=""
AC_ARG_ENABLE([file-transfer],
    AS_HELP_STRING([--disable-file-transfer], [disable file transfer]))
AS_IF([test "x$enable_file_transfer" != "xno"], [
	HAVE_FILE_TRANSFER="-DHAVE_FILE_TRANSFER"
	AC_SUBST(HAVE_FILE_TRANSFER)
])

HAVE_WHITELIST=""
AC_ARG_ENABLE([whitelist],
    AS_HELP_STRING([--disable-whitelist], [disable whitelist]))
AS_IF([test "x$enable_whitelist" != "xno"], [
	HAVE_WHITELIST="-DHAVE_WHITELIST"
	AC_SUBST(HAVE_WHITELIST)
])

HAVE_FATAL_WARNINGS=""
AC_ARG_ENABLE([fatal_warnings],
    AS_HELP_STRING([--enable-fatal-warnings], [-W -Wall -Werror]))
AS_IF([test "x$enable_fatal_warnings" = "xyes"], [
	HAVE_FATAL_WARNINGS="-W -Wall -Werror"
	AC_SUBST(HAVE_FATAL_WARNINGS)
])

BUSYBOX_WORKAROUND="no"
AC_ARG_ENABLE([busybox-workaround],
    AS_HELP_STRING([--enable-busybox-workaround], [enable busybox workaround]))
AS_IF([test "x$enable_busybox_workaround" = "xyes"], [
	BUSYBOX_WORKAROUND="yes"
	AC_SUBST(BUSYBOX_WORKAROUND)
])


HAVE_GCOV=""
AC_ARG_ENABLE([gcov],
    AS_HELP_STRING([--enable-gcov], [Gcov instrumentation]))
AS_IF([test "x$enable_gcov" = "xyes"], [
	HAVE_GCOV="--coverage -DHAVE_GCOV "
	EXTRA_LDFLAGS+=" -lgcov --coverage "
	AC_SUBST(HAVE_GCOV)
])


HAVE_GIT_INSTALL=""
AC_ARG_ENABLE([git-install],
    AS_HELP_STRING([--enable-git-install], [enable git install feature]))
AS_IF([test "x$enable_git_install" = "xyes"], [
	HAVE_GIT_INSTALL="-DHAVE_GIT_INSTALL"
	AC_SUBST(HAVE_GIT_INSTALL)
])

HAVE_CONTRIB_INSTALL="yes"
AC_ARG_ENABLE([contrib-install],
    AS_HELP_STRING([--enable-contrib-install], [install contrib scripts]))
AS_IF([test "x$enable_contrib_install" = "xno"],
	[HAVE_CONTRIB_INSTALL="no"],
	[HAVE_CONTRIB_INSTALL="yes"]
)
AC_SUBST(HAVE_CONTRIB_INSTALL)

# checking pthread library
AC_CHECK_LIB([pthread], [main], [], AC_MSG_ERROR([*** POSIX thread support not installed ***]))
AC_CHECK_HEADER(pthread.h,,AC_MSG_ERROR([*** POSIX thread support not installed ***]))
AC_CHECK_HEADER([linux/seccomp.h], HAVE_SECCOMP_H="-DHAVE_SECCOMP_H", HAVE_SECCOMP_H="")
AC_SUBST(HAVE_SECCOMP_H)

# set sysconfdir
if test "$prefix" = /usr; then
	sysconfdir="/etc"
fi

AC_OUTPUT(Makefile src/common.mk src/lib/Makefile src/fcopy/Makefile src/fnet/Makefile src/firejail/Makefile src/fnetfilter/Makefile \
src/firemon/Makefile src/libtrace/Makefile src/libtracelog/Makefile src/firecfg/Makefile src/fbuilder/Makefile src/fsec-print/Makefile \
src/ftee/Makefile src/faudit/Makefile src/fseccomp/Makefile src/fldd/Makefile src/libpostexecseccomp/Makefile src/fsec-optimize/Makefile)

echo
echo "Configuration options:"
echo "   prefix: $prefix"
echo "   sysconfdir: $sysconfdir"
echo "   seccomp: $HAVE_SECCOMP"
echo "   <linux/seccomp.h>: $HAVE_SECCOMP_H"
echo "   apparmor: $HAVE_APPARMOR"
echo "   global config: $HAVE_GLOBALCFG"
echo "   chroot: $HAVE_CHROOT"
echo "   bind: $HAVE_BIND"
echo "   network: $HAVE_NETWORK"
echo "   user namespace: $HAVE_USERNS"
echo "   X11 sandboxing support: $HAVE_X11"
echo "   whitelisting: $HAVE_WHITELIST"
echo "   private home support: $HAVE_PRIVATE_HOME"
echo "   file transfer support: $HAVE_FILE_TRANSFER"
echo "   overlayfs support: $HAVE_OVERLAYFS"
echo "   git install support: $HAVE_GIT_INSTALL"
echo "   busybox workaround: $BUSYBOX_WORKAROUND"
echo "   Spectre compiler patch: $HAVE_SPECTRE"
echo "   EXTRA_LDFLAGS: $EXTRA_LDFLAGS"
echo "   EXTRA_CFLAGS: $EXTRA_CFLAGS"
echo "   fatal warnings: $HAVE_FATAL_WARNINGS"
echo "   Gcov instrumentation: $HAVE_GCOV"
echo "   Install contrib scripts: $HAVE_CONTRIB_INSTALL"
echo
