default: build

build: cmdtocanonij2/filter/cmdtocanonij2 cnijbe2/src/cnijbe2 ../lgmon3/src/cnijlgmon3 rastertocanonij/src/rastertocanonij tocanonij/src/tocanonij tocnpwg/src/tocnpwg

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

install: cmdtocanonij2/filter/cmdtocanonij2 cnijbe2/src/cnijbe2 $(WRKSRC)/lgmon3/src/cnijlgmon3 rastertocanonij/src/rastertocanonij tocanonij/src/tocanonij tocnpwg/src/tocnpwg
	mkdir -pv $(DESTDIR)$(LIBDIR)/bjlib2/
	mkdir -pv $(DESTDIR)$(DATADIR)/cups/model/
	install -v -c -m 644 $(WRKSRC)/com/ini/cnnet.ini $(DESTDIR)$(LIBDIR)/bjlib2/
	install -v -c -s -m 755 $(WRKSRC)/com/libs_bin$(ARC)/*.so.* $(DESTDIR)$(LIBDIR)
	install -v -m 644 $(WRKSRC)/ppd/*.ppd $(DESTDIR)$(DATADIR)/cups/model/
	@# DESTDIR is forwarded to these:
	$(MAKE) -C cmdtocanonij2 install
	$(MAKE) -C cnijbe2 install
	$(MAKE) -C $(WRKSRC)/lgmon3 install
	$(MAKE) -C rastertocanonij install
	$(MAKE) -C tocanonij install
	$(MAKE) -C tocnpwg install

clean:
	rm -f cmdtocanonij2/config.status cnijbe2/config.status $(WRKSRC)/lgmon3/config.status rastertocanonij/config.status tocanonij/config.status

configure: cmdtocanonij2/config.status

cmdtocanonij2/filter/cmdtocanonij2: cmdtocanonij2/config.status $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so
	$(MAKE) -C cmdtocanonij2

cnijbe2/src/cnijbe2: cnijbe2/config.status
	$(MAKE) -C cnijbe2

$(WRKSRC)/lgmon3/src/cnijlgmon3: $(WRKSRC)/lgmon3/config.status $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so
	$(MAKE) -C $(WRKSRC)/lgmon3

rastertocanonij/src/rastertocanonij: rastertocanonij/config.status
	$(MAKE) -C rastertocanonij

tocanonij/src/tocanonij: tocanonij/config.status
	$(MAKE) -C tocanonij

tocnpwg/src/tocnpwg: tocnpwg/config.status
	$(MAKE) -C tocnpwg

cmdtocanonij2/config.status:
	mkdir -pv cmdtocanonij2
	cd cmdtocanonij2 && ../../cmdtocanonij2/autogen.sh --prefix=$(PREFIX) --datadir=$(DATADIR) LDFLAGS="-L../../../com/libs_bin$(ARC)"

cnijbe2/config.status:
	mkdir -pv cnijbe2
	cd cnijbe2 && ../../cnijbe2/autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) LDFLAGS="$(CUPS_LDFLAGS)"

$(WRKSRC)/lgmon3/config.status:
	cd ../lgmon3 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC) $(CUPS_LDFLAGS)"

rastertocanonij/config.status:
	mkdir -pv rastertocanonij
	cd rastertocanonij && ../../rastertocanonij/autogen.sh --prefix=$(PREFIX) --enable-progpath=$(BINDIR)

tocanonij/config.status:
	mkdir -pv tocanonij
	cd tocanonij && ../../tocanonij/autogen.sh --prefix=$(PREFIX)

tocnpwg/config.status:
	mkdir -pv tocnpwg
	cd tocnpwg && ../../tocnpwg/autogen.sh --prefix=$(PREFIX)

$(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so.5.0.0
	ln -sv libcnbpcnclapicom2.so.5.0.0 $@

$(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so.1.0.0
	ln -sv libcnbpnet30.so.1.0.0 $@

$(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so.1.2.4
	ln -sv libcnnet2.so.1.2.4 $@

test:
	LD_LIBRARY_PATH=/compat/linux/usr/lib64/:$(WRKSRC)/cnijfilter2/com/libs_bin64 cmdtocanonij2/filter/cmdtocanonij2

.PHONY: default build clean install test
