#
# Copyright (C) 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
# 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016,
# 2017, 2018, 2019, 2020, 2021 Free Software Foundation, Inc.
#
# This file is part of GNU Inetutils.
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

@PATHDEFS_MAKE@

AM_CPPFLAGS = \
	$(iu_INCLUDES) \
	-I$(top_srcdir) \
	$(INCAUTH) $(NCURSES_INCLUDE) \
	$(PATHDEF_DEV) $(PATHDEF_TTY) $(PATHDEF_TTY_PFX) $(PATHDEF_LOGIN)

LDADD = \
	$(top_builddir)/libtelnet/libtelnet.a \
	$(iu_LIBRARIES) \
	$(LIBTERMCAP) $(LIBUTIL) $(PTY_LIB)

inetdaemondir = @inetdaemondir@

inetdaemon_PROGRAMS = $(telnetd_BUILD)

EXTRA_PROGRAMS = telnetd

telnetd_SOURCES = telnetd.c slc.c pty.c term.c utility.c state.c termstat.c

noinst_HEADERS = telnetd.h
