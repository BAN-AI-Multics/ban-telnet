#
# Copyright (C) 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015,
# 2016, 2017, 2018, 2019, 2020, 2021 Free Software Foundation, Inc.
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

AM_CPPFLAGS = $(iu_INCLUDES)

LDADD = $(iu_LIBRARIES)

noinst_PROGRAMS = identify
identify_LDADD = $(top_builddir)/lib/libgnu.a $(LIBUTIL) $(PTY_LIB)

check_PROGRAMS = localhost readutmp runtime-ipv6 test-snprintf waitdaemon

dist_check_SCRIPTS = utmp.sh

TESTS = localhost test-snprintf waitdaemon $(dist_check_SCRIPTS)

TESTS_ENVIRONMENT = EXEEXT=$(EXEEXT)

EXTRA_DIST = tools.sh.in ifconfig_modes.sh

BUILT_SOURCES = tools.sh

CLEANFILES = tools.sh

tools_subst = sed -e 's,[@]GREP[@],$(GREP),g' \
	      -e 's,[@]EGREP[@],$(EGREP),g' \
	      -e 's,[@]FGREP[@],$(FGREP),g' \
	      -e 's,[@]SED[@],$(SED),g' \
	      -e 's,[@]DD[@],$(DD),g' \
	      -e 's,[@]MKTEMP[@],$(MKTEMP),g' \
	      -e 's,[@]NETSTAT[@],$(NETSTAT),g' \
	      -e 's,[@]TARGET[@],$(TARGET),g' \
	      -e 's,[@]TARGET6[@],$(TARGET6),g' \
	      -e 's,[@]TEST_IPV4[@],$(TEST_IPV4),g' \
	      -e 's,[@]TEST_IPV6[@],$(TEST_IPV6),g'

tools.sh: tools.sh.in Makefile
	$(tools_subst) < $(srcdir)/tools.sh.in > $@
