default: libcnijfilter2

libcnijfilter2: cmdtocanonij2/filter/cmdtocanonij2 cnijbe2/src/cnijbe2 #liblgmon3 librastertocanonij libtocanonij libtocnpwg

# Dependencies
# - linux_base-rl9
# - cups

PREFIX=/usr/local
DATADIR=$(PREFIX)/share
BINDIR=$(PREFIX)/bin
ARC=64
SRCDIR=..

# TODO get from pkg-config --libs:
CUPS_LDFLAGS=-L/usr/local/lib -lcups

clean:
	rm -f cmdtocanonij2/config.status cnijbe2/config.status

configure: cmdtocanonij2/config.status

cmdtocanonij2/filter/cmdtocanonij2: cmdtocanonij2/config.status $(SRCDIR)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so
	$(MAKE) -C cmdtocanonij2

cnijbe2/src/cnijbe2: cnijbe2/config.status
	$(MAKE) -C cnijbe2

cmdtocanonij2/config.status:
	mkdir -pv cmdtocanonij2
	(cd cmdtocanonij2 && ../../cmdtocanonij2/autogen.sh --prefix=$(PREFIX) --datadir=$(DATADIR) LDFLAGS="-L../../../com/libs_bin$(ARC)")

cnijbe2/config.status:
	mkdir -pv cnijbe2
	(cd cnijbe2 && ../../cnijbe2/autogen.sh --prefix=$(PREFIX) --enable-progpath=$(BINDIR) LDFLAGS="$(CUPS_LDFLAGS)")

$(SRCDIR)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so: $(SRCDIR)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so.5.0.0
	ln -sv libcnbpcnclapicom2.so.5.0.0 $@

test:
	LD_LIBRARY_PATH=/compat/linux/usr/lib64/:$(SRCDIR)/cnijfilter2/com/libs_bin64 cmdtocanonij2/filter/cmdtocanonij2
