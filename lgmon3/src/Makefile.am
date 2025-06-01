## Process this file with automake to produce Makefile.in

#cnijlgmon2_bindir = /usr/local/bin
bin_PROGRAMS = cnijlgmon3
transform = 

cnijlgmon3_SOURCES = cnijcomif.c cnijifusb.c cnijlgmon3.c cnijifnet.c keytext.c cnijifnet2.c cnijutil.c

CFLAGS = -O2 -Wall -I/usr/include/libusb-1.0 -I./common @XML2_CFLAGS@ @CUPS_CFLAGS@
#LDFLAGS = @XML2_LDFLAGS@

cnijlgmon3_LDADD = -ldl @XML2_LDFLAGS@ @USB1_LIBS@ -lcnnet2 -lcnbpcnclapicom2 -lcnbpnet30 -lintl
