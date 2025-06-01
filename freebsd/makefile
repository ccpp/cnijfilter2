default: libcnijfilter2

libcnijfilter2: cmdtocanonij2/filter/cmdtocanonij2 cnijbe2/src/cnijbe2 ./lgmon3/src/cnijlgmon3 #librastertocanonij libtocanonij libtocnpwg

# Dependencies
# - linux_base-rl9
# - cups

PREFIX=/usr/local
DATADIR=$(PREFIX)/share
BINDIR=$(PREFIX)/bin
LIBDIR=$(PREFIX)/lib
ARC=64
WRKSRC=..

# TODO get from pkg-config --libs:
CUPS_LDFLAGS=-L/usr/local/lib -lcups

clean:
	rm -f cmdtocanonij2/config.status cnijbe2/config.status $(WRKSRC)/lgmon3/config.status

configure: cmdtocanonij2/config.status

cmdtocanonij2/filter/cmdtocanonij2: cmdtocanonij2/config.status $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so
	$(MAKE) -C cmdtocanonij2

cnijbe2/src/cnijbe2: cnijbe2/config.status
	$(MAKE) -C cnijbe2

./lgmon3/src/cnijlgmon3: $(WRKSRC)/lgmon3/config.status $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so
	$(MAKE) -C $(WRKSRC)/lgmon3

cmdtocanonij2/config.status:
	mkdir -pv cmdtocanonij2
	(cd cmdtocanonij2 && ../../cmdtocanonij2/autogen.sh --prefix=$(PREFIX) --datadir=$(DATADIR) LDFLAGS="-L../../../com/libs_bin$(ARC)")

cnijbe2/config.status:
	mkdir -pv cnijbe2
	(cd cnijbe2 && ../../cnijbe2/autogen.sh --prefix=$(PREFIX) --enable-progpath=$(BINDIR) LDFLAGS="$(CUPS_LDFLAGS)")

$(WRKSRC)/lgmon3/config.status:
	(cd ../lgmon3 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC) $(CUPS_LDFLAGS)")

$(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so.5.0.0
	ln -sv libcnbpcnclapicom2.so.5.0.0 $@

$(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so.1.0.0
	ln -sv libcnbpnet30.so.1.0.0 $@

$(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so.1.2.4
	ln -sv libcnnet2.so.1.2.4 $@

test:
	LD_LIBRARY_PATH=/compat/linux/usr/lib64/:$(WRKSRC)/cnijfilter2/com/libs_bin64 cmdtocanonij2/filter/cmdtocanonij2
