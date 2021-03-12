# configure.ac - Configuration for BAN telnet
# Copyright (C) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004,
# 2005, 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015,
# 2016, 2017, 2018, 2019, 2020, 2021 Free Software Foundation, Inc.
#
# This is BAN telnet, based on GNU Inetutils.
#
# The following applies to BAN telnet as well as GNU Inetutils:
#
# GNU Inetutils is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or (at
# your option) any later version.
#
# GNU Inetutils is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see `http://www.gnu.org/licenses/'.

AC_PREREQ(2.64)

AC_INIT([BAN telnet],
 m4_esyscmd([build-aux/git-version-gen .tarball-version]),
 [trnsz@pobox.com])

AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_HEADERS([config.h:config.hin])
AC_CANONICAL_HOST

AM_INIT_AUTOMAKE([1.11.1 dist-xz -Wall -Werror])

# For now, we always use libexecdir; later we may want to vary where
# the daemons go.
inetdaemondir='$(libexecdir)'
AC_SUBST(inetdaemondir)

AC_ARG_WITH([idn],
  AS_HELP_STRING([--with-idn[[=DIR]]],
		 [Use IDN or IDN2 support, with optional inclusion directory]), ,
		 [with_idn=no])
AC_ARG_WITH([libidn-prefix],
  AS_HELP_STRING([--with-libidn-prefix[[=DIR]]],
		 [Base directory, probes DIR/include and DIR/lib]), ,
		 [with_libidn_prefix=no])

AC_ARG_ENABLE([servers],
              AS_HELP_STRING([--disable-servers],
                             [Do not compile server]),
          , [enable_servers=yes])
AC_ARG_ENABLE([clients],
              AS_HELP_STRING([--disable-clients],
                             [Do not compile client]),
          , [enable_clients=yes])

IU_ENABLE_SERVER(telnetd)
IU_ENABLE_CLIENT(telnet)

### Checks for programs.
AC_PROG_CC
gl_EARLY
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])
AC_CHECK_TOOL(AR, ar)
AC_PATH_PROG(RM, rm, rm)
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_MAKE_SET
AC_PROG_RANLIB
AC_PROG_YACC
AC_PROG_LN_S
AC_ARG_VAR(MKTEMP, [Location of 'mktemp'.])
AC_ARG_VAR(TARGET, [IP address used while testing. @<:@127.0.0.1@:>@])
AC_ARG_VAR(TARGET6, [IPv6 address used while testing. @<:@::1@:>@])

gl_INIT

### Checks for libraries.
AC_SEARCH_LIBS([inet_ntoa], [nsl])
AC_SEARCH_LIBS([getpeername], [socket])
IU_LIB_TERMCAP

# libutil is a 4.4BSD library that defines several functions handy for
# inetutils daemons, doing such things as utmp frobbing, and pty
# setup.
AC_CHECK_LIB(util, login, LIBUTIL=-lutil)
AC_CHECK_LIB(util, loginx, LIBUTIL=-lutil)
AC_CHECK_LIB(util, logout, LIBUTIL=-lutil)
AC_CHECK_LIB(util, logoutx, LIBUTIL=-lutil)
AC_CHECK_LIB(util, logwtmp, LIBUTIL=-lutil)
AC_CHECK_LIB(util, logwtmpx, LIBUTIL=-lutil)
AC_SUBST(LIBUTIL)

# Make IPv4 tests configurable in testing stage.

AC_ARG_ENABLE([ipv4],
  AS_HELP_STRING([--disable-ipv4], [disable IPv4 testing]),
  [case "${enable_ipv4}" in
  no)
    AC_MSG_NOTICE([Disabling IPv4 tests at user request])
    ipv4=no
    ;;
  *)
    ipv4=yes
    ;;
  esac],
  [ipv4=auto]
)
AC_SUBST(TEST_IPV4, [$ipv4])

# Checks for IPv6 support.

AC_ARG_ENABLE([ipv6],
  AS_HELP_STRING([--disable-ipv6], [disable IPv6 support]),
  [case "${enable_ipv6}" in
  no)
    AC_MSG_NOTICE([Disabling IPv6 at user request])
    ipv6=no
    ;;
  *)
    ipv6=yes
    ;;
  esac],
  [ipv6=auto]
)
AC_SUBST(TEST_IPV6, [$ipv6])

working_ipv6=yes
if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  IU_CHECK_MACRO(AF_INET6, [#include <sys/socket.h>], , , working_ipv6=no)
fi

if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  IU_CHECK_MACRO(IPV6_V6ONLY, [#include <netinet/in.h>], , , working_ipv6=no)
fi

if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  AC_CHECK_TYPE(struct sockaddr_storage, , working_ipv6=no, [#include <sys/socket.h>])
fi

if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  AC_CHECK_TYPE(struct sockaddr_in6, , working_ipv6=no, [#include <sys/types.h>
#include <netinet/in.h>])
fi

if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  AC_CHECK_TYPE(struct addrinfo, , working_ipv6=no, [#include <netdb.h>])
fi

if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  AC_CHECK_FUNC(getnameinfo, ,working_ipv6=no, [#include <netdb.h>])
fi

if test ! "$ipv6" = "no" && test "$working_ipv6" = "yes"; then
  AC_DEFINE([IPV6], 1, [Define to one if you want IPv6.])
else
  if test "$ipv6" = "yes"; then
    AC_MSG_FAILURE([IPv6 support not available])
  else
    if test "$ipv6" = "auto"; then
      AC_MSG_WARN([IPv6 support not available, disabling IPv6])
    fi
  fi
fi

### Check for GNU Libidn
AC_CHECK_HEADERS([locale.h])
AC_CHECK_FUNCS([setlocale])

IU_CHECK_LIBIDN([$with_libidn_prefix], [$with_idn])

### Checks for header files.

AC_HEADER_STDC
AC_HEADER_TIME
AC_HEADER_DIRENT

AC_CHECK_HEADERS([arpa/nameser.h arpa/tftp.h errno.h fcntl.h features.h \
		  glob.h memory.h netinet/ether.h netinet/in_systm.h \
		  netinet/ip.h netinet/ip_icmp.h netinet/ip_var.h \
		  security/pam_appl.h shadow.h \
		  stdarg.h stdlib.h string.h stropts.h sys/tty.h \
		  sys/utsname.h sys/ptyvar.h sys/msgbuf.h sys/filio.h \
		  sys/ioctl_compat.h sys/cdefs.h sys/stream.h sys/mkdev.h \
		  sys/sockio.h sys/sysmacros.h sys/param.h sys/file.h \
		  sys/proc.h sys/select.h sys/time.h sys/wait.h \
                  sys/resource.h \
		  stropts.h tcpd.h utmp.h utmpx.h unistd.h \
                  vis.h], [], [], [
#include <sys/types.h>
#ifdef HAVE_NETINET_IN_SYSTM_H
# include <netinet/in_systm.h>
#endif
#include <netinet/in.h>
#ifdef HAVE_NETINET_IP_H
# include <netinet/ip.h>
#endif
#ifdef HAVE_SYS_PARAM_H
# include <sys/param.h>
#endif
]
)

### Checks for types.

# Checks return type of signal handlers
AC_TYPE_SIGNAL

AC_CHECK_TYPE(socklen_t, ,
  [AC_DEFINE([socklen_t], int,
    [Define to int if <sys/types.h> does not define])],
  [IU_FLUSHLEFT([#include <sys/types.h>
                 #include <sys/socket.h>])])

AC_CHECK_TYPES(struct if_nameindex, , ,
  [IU_FLUSHLEFT([#include <sys/types.h>
                 #include <sys/socket.h>
                 #include <net/if.h>])])

# sigset_t is a set of signals (4.4BSD)
AC_CHECK_TYPE(sigset_t, ,
  [AC_DEFINE([sigset_t], unsigned long,
    [Define to `unsigned long' if not defined in <signal.h>])],
  [IU_FLUSHLEFT([#include <sys/types.h>
                 #include <signal.h> ])])

# I'm not sure what this is, but netbsd has it; just use int if
# necessary.
AC_CHECK_TYPE(sig_atomic_t, ,
  [AC_DEFINE([sig_atomic_t], int,
    [Define to `int' if not defined in <signal.h>])],
  [IU_FLUSHLEFT([#include <sys/types.h>
                 #include <signal.h> ])])

### Checks for structures.
IU_CHECK_MEMBERS([struct sockaddr_in.sin_len], , ,
    [ #include <sys/types.h>
      #include <netinet/in.h> ])
IU_CHECK_MEMBERS([struct sockaddr_in6.sin6_len], , ,
    [ #include <sys/types.h>
      #include <sys/socket.h>
      #include <netinet/in.h> ])
IU_CHECK_MEMBERS([struct sockaddr_storage.ss_len], , ,
    [ #include <sys/types.h>
      #include <sys/socket.h>
      #include <netinet/in.h> ])

IU_CHECK_MEMBERS([struct passwd.pw_expire, struct passwd.pw_change],
    [], [],
    [ #include <sys/types.h>
      #include <pwd.h>])

if test "$ac_cv_header_utmp_h" = yes; then
  AC_CHECK_TYPES(struct lastlog, , , [#include <utmp.h>])
  IU_CHECK_MEMBERS([struct utmp.ut_type, struct utmp.ut_pid,
		    struct utmp.ut_line, struct utmp.ut_id,
		    struct utmp.ut_user, struct utmp.ut_host,
		    struct utmp.ut_exit, struct utmp.ut_session,
		    struct utmp.ut_tv, struct utmp.ut_time,
		    struct utmp.ut_addr_v6, struct utmp.ut_name], , ,
		   [#include <sys/types.h>
		    #include <utmp.h>])
fi
if test "$ac_cv_header_utmpx_h" = yes; then
  AC_CHECK_TYPES(struct lastlogx, , , [#include <utmp.h>])
  IU_CHECK_MEMBERS([struct lastlogx.ll_ss], , ,
		   [#include <sys/types.h>
		    #include <sys/utmpx.h>])
  IU_CHECK_MEMBERS([struct utmpx.ut_user, struct utmpx.ut_name,
		    struct utmpx.ut_id, struct utmpx.ut_line,
		    struct utmpx.ut_pid, struct utmpx.ut_type,
		    struct utmpx.ut_exit, struct utmpx.ut_tv,
		    struct utmpx.ut_session, struct utmpx.ut_syslen,
		    struct utmpx.ut_host, struct utmpx.ut_ss,
		    struct utmpx.ut_addr_v6], , , [#include <utmpx.h>])
fi

IU_CHECK_MEMBERS([struct ifreq.ifr_index,
		  struct ifreq.ifr_netmask,
		  struct ifreq.ifr_broadaddr,
		  struct ifreq.ifr_mtu,
		  struct ifreq.ifr_map], , ,
		 [#include <sys/types.h>
		  #include <sys/socket.h>
		  #include <net/if.h>])

IU_CHECK_MEMBERS([struct sockaddr.sa_len], , ,
                 [#include <sys/types.h>
                  #include <sys/socket.h>])
IU_CHECK_MEMBERS([struct hostent.h_addr_list], , , [#include <netdb.h>])

IU_CHECK_MEMBERS([struct stat.st_atim.tv_nsec,
		  struct stat.st_atim.tv_usec,
		  struct stat.st_blksize,
		  struct stat.st_ctim.tv_nsec,
		  struct stat.st_ctim.tv_usec,
		  struct stat.st_mtim.tv_nsec,
		  struct stat.st_mtim.tv_usec], , ,
		 [#include <sys/types.h>
		  #include <sys/stat.h>])

### Checks for compiler characteristics.
AC_C_CONST

### Checks for library functions.
AC_FUNC_CLOSEDIR_VOID
AC_FUNC_SETVBUF_REVERSED
AC_FUNC_FORK
AC_FUNC_STRCOLL
AC_FUNC_MMAP

AC_CHECK_FUNCS(cfsetspeed cgetent dirfd fchdir flock \
               fork fpathconf ftruncate \
               getcwd getmsg getpwuid_r getspnam getutxent getutxuser \
               initgroups initsetproctitle killpg \
               ptsname pututline pututxline \
               setegid seteuid setpgid setlogin \
               setsid setregid setreuid setresgid setresuid setutent_r \
               sigaction sigvec strchr setproctitle tcgetattr tzset utimes \
               utime uname \
               updwtmp updwtmpx vhangup wait3 wait4 __opendir2 \
	       __rcmd_errstr __check_rhosts_file )

# Variant functions for user accounting.
# These need $LIBUTIL for linking.
_SAVE_LIBS="$LIBS"
LIBS="$LIBS $LIBUTIL"
AC_CHECK_FUNCS(login loginx logout logoutx logwtmp logwtmpx)
LIBS="$_SAVE_LIBS"

# Functions that we will define if necessary.
AC_REPLACE_FUNCS(getpass getusershell memcmp memcpy memmove memset \
		 strdup poll)

AC_CHECK_FUNC(_obstack_free)

# Use (posix) `setpgid' if it's around, otherwise assume a BSD setpgrp
AC_CHECK_FUNC(setpgid, , AC_DEFINE([setpgid], setpgrp,
                          [Define to `setpgrp' if you don't have `setpgid'.]))

# We use our own version of getopt (including our own header file) if
# the system one doesn't have getopt_long.
AC_CHECK_FUNC(getopt_long)

AC_CHECK_HEADER(poll.h)

# See if snprintf exists, otherwise just use a bogus version
AC_CHECK_FUNC(snprintf,
  [AC_DEFINE([HAVE_SNPRINTF], 1, [Define to one if you have snprintf])])

# See if vsnprintf exists, otherwise use our own.
AC_CHECK_FUNC(vsnprintf,
  [AC_DEFINE([HAVE_VSNPRINTF], 1, [Define to one if you have vsnprintf])])

# See if the system has strerror, and replace it if not
AC_CHECK_FUNC(strerror)
if test "$ac_cv_func_strerror" = no; then
  # No strerror, so see if the SYS_ERRLIST variable can be used by
  # ours
  AC_CHECK_FUNC(sys_errlist, AC_DEFINE([HAVE_SYS_ERRLIST], 1,
                              [Define to one if you have sys_errlist.]))
  if test "$ac_cv_func_sys_errlist" = yes; then
    AC_CHECK_DECL(sys_errlist, , ,
      [IU_FLUSHLEFT([#include <stdio.h>
                    #include <errno.h>])])
  fi
fi

# See if the system has hstrerror, and replace it if not
save_LIBS=$LIBS

AC_CHECK_FUNC(hstrerror)
if test "$ac_cv_func_hstrerror" != yes; then
  # Is hstrerror contained in a separate library?
  AC_CHECK_LIB([resolv], [hstrerror], LIBRESOLV=-lresolv)
  LIBS="$LIBRESOLV $LIBS"
fi
if test "$ac_cv_func_hstrerror" = yes; then
  AC_CHECK_DECL(hstrerror, , , [#include <netdb.h>])
else
  # No hstrerror, so see if the H_ERRLIST variable can be used by ours
  AC_CHECK_FUNC(h_errlist)
  if test "$ac_cv_func_h_errlist" = yes; then
    AC_CHECK_DECL(h_errlist, , , [#include <netdb.h>])
  fi
fi
if test "$ac_cv_func_hstrerror" = yes \
   || test "$ac_cv_func_h_errlist" = yes; then
  # If there's a system hstrerror, or we can reasonably replace it,
  # say so.  We still provide some definition, regardless, but this
  # allows people to use a reasonable alternative if the situation
  # allows, rather than using a degenerate version that only says
  # `Host lookup error N'.
  AC_DEFINE([HAVE_HSTRERROR], 1, [Define to one if you have hstrerror.])
fi
LIBS=$save_LIBS
AC_SUBST(LIBRESOLV)

# OpenSolaris does not provide an external reference.
AC_CHECK_DECLS(telcmds, , ,
  [IU_FLUSHLEFT([#undef TELCMDS
                 #include <arpa/telnet.h>])])

# NetBSD does include an external reference.
AC_CHECK_DECLS(telopts, , ,
  [IU_FLUSHLEFT([#undef TELOPTS
                 #include <arpa/telnet.h>])])

# OpenSolaris provides Schedule and Session_Key.
AC_CHECK_TYPE(Schedule,
  AC_DEFINE([HAVE_ARPA_TELNET_H_SCHEDULE], 1,
	    [Define to one if <arpa/telnet.h> defines a type Schedule.]), ,
  [#include <arpa/telnet.h>])
AC_CHECK_TYPE(Session_Key,
  AC_DEFINE([HAVE_ARPA_TELNET_H_SESSION_KEY], 1,
	    [Define to 1 if <arpa/telnet.h> defines a type Session_Key.]), ,
  [#include <arpa/telnet.h>])

# At least NetBSD demands `const char []'.
AC_MSG_CHECKING([whether enctype_names is const char])
AC_COMPILE_IFELSE(
  [AC_LANG_PROGRAM(
     [[#include <arpa/telnet.h>
       const char *enctype_names[] = { "ANY", "CFB64", 0 };]],
     [[ return 0;]])],
  [AC_DEFINE([HAVE_CONST_CHAR_ENCTYPE_NAMES], 1,
	     [Define to 1 if enctype_names[] must be const char.])
   ac_iu_const_enctype_names=yes],
  [ac_iu_const_enctype_names=no])
AC_MSG_RESULT([$ac_iu_const_enctype_names])

## Checks for function declarations.

AC_DECL_SYS_SIGLIST

# EWOULDBLOCK is more or less the BSD version of posix EAGAIN.
IU_CHECK_MACRO(EWOULDBLOCK, [#include <errno.h>], , ,
  [AC_DEFINE([EWOULDBLOCK], EAGAIN, 1,
     [Define to EAGAIN if system does not provide EWOULDBLOCK])])

AC_CHECK_DECLS(errno, , , [#include <errno.h>])

# See whether <syslog.h> will declare special internal stuff if we
# define SYSLOG_NAMES before including it.  We check for various
# syslog internal macros, the typedef `CODE', and the variable
# `prioritynames'.
define([iu_syslog_includes],
  [[#include <stdlib.h>
    #define SYSLOG_NAMES
    #include <syslog.h> ]])
IU_CHECK_MACRO(syslog internal macros,
  iu_syslog_includes,
  [LOG_PRI LOG_FAC],
  AC_CHECK_TYPE(CODE,
    AC_CHECK_DECL(prioritynames,
      [AC_DEFINE([HAVE_SYSLOG_INTERNAL], 1,
        [Define to 1 if <syslog.h> defines prioritynames])], ,
      iu_syslog_includes), , iu_syslog_includes),)
undefine([iu_syslog_includes])

# Some systems don't declare common functions (especially if they
# return int), at least in the expected header file.  Check.
AC_CHECK_DECLS([fclose, pclose], , , [#include <stdio.h>])
AC_CHECK_DECLS([getcwd, getlogin, getpass, getusershell, ttyname], , ,
               [#include <unistd.h>])

AC_CHECK_DECLS([getgrnam, initgroups], , , [#include <grp.h>])

AC_CHECK_DECLS(htons, , ,
  [IU_FLUSHLEFT([# include <sys/types.h>
                 #ifdef HAVE_SYS_PARAM_H
                 # include <sys/param.h>
                 #endif
                 #include <netinet/in.h> ])])

# Our home-grown version of setenv needs to use ENVIRON
AC_CHECK_DECLS(environ, , ,
  [IU_FLUSHLEFT([#include <unistd.h>
                 #include <stdlib.h>])])

# Look for h_errno (the test is here so we can use -lresolv if necessary).
AC_CACHE_CHECK([for h_errno], inetutils_cv_var_h_errno,
  AC_TRY_COMPILE([#include <netdb.h>],
    [ extern int h_errno; int iu_x = h_errno; ],
    inetutils_cv_var_h_errno=yes, inetutils_cv_var_h_errno=no))
if test "$inetutils_cv_var_h_errno" = yes; then
  # FIXME: Isn't this wrong? Shouldn't the declaration check define
  #        this if we find h_errno?
  AC_DEFINE([HAVE_H_ERRNO], 1, [Define to one if you have h_errno.])
  AC_CHECK_DECL(h_errno, , , [#include <netdb.h>])
fi

## Checks for CPP macros.

# Find posix SEEK_ macros (for lseek), and if not found, try the similar
# berkeley L_ macros; if neither can be found, use the classic unix values.
IU_CHECK_MACRO(SEEK_ macros,
  [#include <unistd.h>], SEEK_SET SEEK_CUR SEEK_END,
  :,
  IU_CHECK_MACRO(L_ seek macros,
    [#include <unistd.h>], L_SET L_INCR L_XTND,
    AC_DEFINE([SEEK_SET], L_SET, [Define to L_SET as replacement])
    AC_DEFINE([SEEK_CUR], L_INCR, [Define to L_INCR as replacement])
    AC_DEFINE([SEEK_END], L_XTND, [Define to L_XTND as replacement]),
    AC_DEFINE([SEEK_SET], 0, [Define to 0 if missing])
    AC_DEFINE([SEEK_CUR], 1, [Define to 1 if missing])
    AC_DEFINE([SEEK_END], 2, [Define to 2 if missing])))

# Look posix _FILENO macros; if not found, use the classic unix values.
IU_CHECK_MACRO(_FILENO macros,
  [#include <unistd.h>], STDIN_FILENO STDOUT_FILENO STDERR_FILENO,
  :,
  AC_DEFINE([STDIN_FILENO], 0, [Define to 0 if missing])
  AC_DEFINE([STDOUT_FILENO], 1, [Define to 1 if missing])
  AC_DEFINE([STDERR_FILENO], 2, [Define to 2 if missing]))

# See where to find fd_set (bit-vectors for select) manipulation macros.
IU_CHECK_MACRO(fd_set macros, [#include <sys/types.h>],
  FD_ZERO FD_CLR FD_SET FD_ISSET)
if test "$inetutils_cv_macro_fd_set_macros" = no; then
  IU_CHECK_MACRO(fd_set macros in <sys/time.h>, [#include <sys/time.h>],
    FD_ZERO FD_CLR FD_SET FD_ISSET)
fi

# Set convenience variables for inetutils.
iu_LIBRARIES='$(top_builddir)/libinetutils/libinetutils.a $(top_builddir)/lib/libgnu.a'
iu_INCLUDES='-I$(top_srcdir)/lib -I$(top_builddir)/lib -I$(top_srcdir)/libinetutils'
AC_SUBST(iu_LIBRARIES)
AC_SUBST(iu_INCLUDES)

PATHDEFS_MAKE=pathdefs.make
PATHS_DEFS=paths.defs
IU_CONFIG_PATHS($srcdir/paths, PATHDEFS_MAKE, PATHS_DEFS)
AH_BOTTOM(
[/* Defaults for PATH_ variables.  */
#include <confpaths.h>])

case "$host" in
*-freebsd9* | *-freebsd10* )
  ;;
*olaris* )
  case "$host" in
  *olaris*10 | *olaris*11 )
    AC_DEFINE([SOLARIS10], 1, [Solaris 10 and newer variants.])
    ;;
  * )
    ;;
  esac
  AC_DEFINE([SOLARIS], 1, [Define to 1 for any Solaris system])
  AC_DEFINE([HAVE_STREAMSPTY], 1,
    [Define to 1 for a system using streams for ptys])
  ;;
*irix* | *hpux* )
  ;;
esac

# Typically the loopback addresses, but can be set otherwise.
TARGET=${TARGET:-127.0.0.1}
TARGET6=${TARGET6:-::1}

AC_CONFIG_FILES([
Makefile
summary.sh
lib/Makefile
libinetutils/Makefile
libtelnet/Makefile
telnet/Makefile
telnetd/Makefile
confpaths.h:confpaths.h.in
])

AC_CONFIG_COMMANDS([summary], [sh ./summary.sh])

# Support for libidn.
idn_link=`expr X"$LIBIDN" : '.*-l\(idn2*\)'`
telnet_PROPS="${telnet_PROPS:+$telnet_PROPS }${LIBIDN:+$idn_link}"

# Termcap variants.
telnet_PROPS="${telnet_PROPS:+$telnet_PROPS }$LIBTERMCAP"
telnetd_PROPS="${telnetd_PROPS:+$telnetd_PROPS }$LIBTERMCAP"

AC_OUTPUT
