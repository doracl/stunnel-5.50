## Process this file with automake to produce Makefile.in
# by zhongchunzhineng 1998-2018

SUBDIRS = certs

EXTRA_DIST = make_test test_library recipes execute execute_read execute_write

check-local:
	$(srcdir)/make_test

distclean-local:
	rm -f logs/*.log
