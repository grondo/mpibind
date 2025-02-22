## Prologue
AC_INIT([mpibind],
	m4_esyscmd_s([git describe --tags | sed s/^v//]))
AC_CONFIG_MACRO_DIRS([config])
AC_CONFIG_AUX_DIR([config])
AC_CONFIG_HEADERS([config/config.h])


## Automake support
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])
AM_INIT_AUTOMAKE([-Wall -Werror foreign subdir-objects])
AM_SILENT_RULES([yes])


## Check for programs 
# pkg-config is discovered through PKG_PROG_PKG_CONFIG, 
# which detects the presence and version of pkg-config itself.
# Need to make sure that pkg.m4 is in ACLOCAL_PATH, e.g, 
# ACLOCAL_PATH=$ACLOCAL_PATH:PKG_CONFIG_ROOT/share/aclocal
# aclocal --print-ac-dir
# Todo: Create pkg-config vars for mpibind
PKG_PROG_PKG_CONFIG

# libtool 
LT_INIT
#AC_DISABLE_SHARED
#AC_DISABLE_STATIC

AC_PROG_CC_C99
AC_PROG_AWK
AC_PROG_LN_S
AC_PROG_MKDIR_P

# Define the host type variable: host
# And its parts: host_cpu, host_vendor, and host_os.
AC_CANONICAL_HOST
#AC_MSG_NOTICE([$host: $host_cpu, $host_vendor, $host_os])

case "${host_os}" in
    darwin*) darwin_os=true ;;
esac
AM_CONDITIONAL([HAVE_DARWIN_OS], [test x$darwin_os = xtrue])


PKG_CHECK_MODULES([HWLOC], [hwloc >= 2.1], [
  hwloc_dir=`$PKG_CONFIG --variable=libdir hwloc`
  AC_MSG_NOTICE([$hwloc_dir])
  ]
)

# In newer versions of pkgconf, I could use
# PKG_HAVE_WITH_MODULES and PKG_CHECK_VAR.
# I could also request min-version above with
# PKG_PROG_PKG_CONFIG([MIN-VERSION])
PKG_CHECK_MODULES([TAP], [tap], [
  libtap=true
  libtap_dir=`$PKG_CONFIG --variable=libdir tap`
  AC_SUBST(TAP_LIBDIR, $libtap_dir)
  AC_REQUIRE_AUX_FILE([tap-driver.sh])
  AC_MSG_NOTICE([$libtap_dir])
  ],
  [AC_MSG_NOTICE([Test suite will not be built])]
)
AM_CONDITIONAL([HAVE_LIBTAP], [test x$libtap = xtrue])
# PKG_CHECK_VAR([TAP_LIBDIR], [tap], [libdir],
#   [libtap_libdir=true],
#   [AC_MSG_NOTICE([TAP's libdir not found])]
# )

PKG_CHECK_MODULES([FLUX_CORE], [flux-core], [
  flux_core=true
  flux_dir=`$PKG_CONFIG --variable=libdir flux-core`
  AC_MSG_NOTICE([$flux_dir])
  ],
  [AC_MSG_NOTICE([Flux plugin will not be built])]
)
AM_CONDITIONAL([HAVE_FLUX_CORE], [test x$flux_core = xtrue])
# Flux should install the mpibind plugin rather than mpibind.
#PKG_CHECK_MODULES([FLUX_CORE], [flux-core], [
#  fluxcore=true
#  flux_shell_plugin_dir=`$PKG_CONFIG --variable=libdir flux-core`
#  flux_shell_plugin_dir+=/flux/shell/plugins
#  AC_SUBST(FLUX_SHELL_PLUGIN_DIR, $flux_shell_plugin_dir)
#  ],
#  [AC_MSG_NOTICE([Flux plugin will not be built])]
#)
#AM_CONDITIONAL([HAVE_FLUX_CORE], [test x$fluxcore = xtrue])

# Notes 
# AC_DEFINE([HAVE_LIBTAP], 1, [Define libtap to build the test suite])
# AC_CHECK_FUNC([hwloc_topology_set_all_types_filter])
# AC_CHECK_HEADERS([pkgconf/libpkgconf/libpkgconf.h])
# defines HAVE_LIBTAP and prepends -ltap to LIBS
# AC_CHECK_LIB(tap, plan)
# AC_SEARCH_LIBS([pkgconf_pkg_free], [pkgconf],
#  [AC_MSG_NOTICE([Greetings from pkgconf])],
#  [AC_MSG_ERROR([unable to find pkgconf_pkg_free()])
# ])
# AS_IF(test-1, [run-if-true-1], ..., [run-if-false])

# Define pkgconfigdir to install mpibind.pc
PKG_INSTALLDIR

# Path for mpibind modules, e.g., flux plugin
AS_VAR_SET(mpibindmoddir, $libdir/mpibind)
AC_SUBST(mpibindmoddir)

## Epilogue
AC_CONFIG_FILES([
 Makefile
 src/Makefile
 test-suite/Makefile
 flux/Makefile
 etc/Makefile
 etc/mpibind.pc
])

AC_OUTPUT
