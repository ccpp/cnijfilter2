
INCLUDES = \
	@XML_2_CFLAGS@

bin_PROGRAMS= tocnpwg

tocnpwg_SOURCES= \
	main.c mkpset.c

tocnpwg_LDADD= @CUPS_LIBS@ -lcupsimage -lxml2

AM_CFLAGS= -O2 -Wall @CUPS_CFLAGS@

