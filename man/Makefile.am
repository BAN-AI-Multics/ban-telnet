#
# Copyright (C) 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
# 2018, 2019, 2020, 2021 Free Software Foundation, Inc.
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

all = telnet.1 telnetd.8

dist_man_MANS =

if ENABLE_telnet
dist_man_MANS += telnet.1
endif

if ENABLE_telnetd
dist_man_MANS += telnetd.8
endif

EXTRA_DIST = $(all) $(all:.1=.h2m) $(all:.8=.h2m)

MAINTAINERCLEANFILES = $(all)

telnet.1: telnet.h2m $(top_srcdir)/telnet/main.c $(top_srcdir)/configure.ac

telnetd.8: telnetd.h2m $(top_srcdir)/telnetd/telnetd.c $(top_srcdir)/configure.ac

.h2m.1:
	$(MAKE) man TOOL=$* SECTION=1

.h2m.8:
	$(MAKE) man TOOL=$* SECTION=8

.PHONY: man
man:
	$(HELP2MAN)					\
		--source='$(PACKAGE_STRING)'		\
		--include=$(srcdir)/$(TOOL).h2m		\
		--output=$(TOOL).$(SECTION)		\
		--section $(SECTION)			
