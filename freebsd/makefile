default: build
	@echo "Now type 'make install DESTDIR=(destinatination)' to install"

# Dependencies
# - linux_base-rl9
# - cups

#DESTDIR=/
PREFIX=/usr/local
DATADIR=$(PREFIX)/share
BINDIR=$(PREFIX)/bin
LIBDIR=$(PREFIX)/lib
ETCDIR=$(PREFIX)/etc
ARC=64
WRKSRC=..
#CUPSBINDIR=$(PREFIX)/libexec/cups # cups-config --serverbin

# TODO get from pkg-config --libs:
CUPS_LDFLAGS=-L/usr/local/lib -lcups

install: build
	mkdir -pv $(DESTDIR)$(LIBDIR)/bjlib2/
	mkdir -pv $(DESTDIR)$(DATADIR)/cups/model/
	mkdir -pv $(DESTDIR)$(ETCDIR)/libmap.d/
	install -v -c -m 644 $(WRKSRC)/com/ini/cnnet.ini $(DESTDIR)$(LIBDIR)/bjlib2/
	install -v -c -s -m 755 $(WRKSRC)/com/libs_bin$(ARC)/*.so.* $(DESTDIR)$(LIBDIR)
	ln -svf libcnbpcnclapicom2.so.5.0.0 $(DESTDIR)$(LIBDIR)/libcnbpcnclapicom2.so
	ln -svf libcnbpnet30.so.1.0.0 $(DESTDIR)$(LIBDIR)/libcnbpnet30.so
	ln -svf libcnnet2.so.1.2.4 $(DESTDIR)$(LIBDIR)/libcnnet2.so
	install -v -m 644 $(WRKSRC)/ppd/*.ppd $(DESTDIR)$(DATADIR)/cups/model/
	install -v -m 644 cnijfilter2.x64.libmap.conf ${DESTDIR}${ETCDIR}/libmap.d/
	@# DESTDIR is forwarded to these:
	$(MAKE) -C $(WRKSRC)/cmdtocanonij2 install
	$(MAKE) -C $(WRKSRC)/cnijbe2 install
	$(MAKE) -C $(WRKSRC)/lgmon3 install
	$(MAKE) -C $(WRKSRC)/rastertocanonij install
	$(MAKE) -C $(WRKSRC)/tocanonij install
	$(MAKE) -C $(WRKSRC)/tocnpwg install

clean:
	-$(MAKE) -C $(WRKSRC)/cmdtocanonij2 clean
	-$(MAKE) -C $(WRKSRC)/cnijbe2 clean
	-$(MAKE) -C $(WRKSRC)/lgmon3 clean
	-$(MAKE) -C $(WRKSRC)/rastertocanonij clean
	-$(MAKE) -C $(WRKSRC)/tocanonij clean
	-$(MAKE) -C $(WRKSRC)/tocnpwg clean
	#rm -f $(WRKSRC)/cmdtocanonij2/Makefile $(WRKSRC)/cnijbe2/Makefile $(WRKSRC)/lgmon3/Makefile $(WRKSRC)/rastertocanonij/Makefile $(WRKSRC)/tocanonij/Makefile $(WRKSRC)/tocnpwg/Makefile

build $(WRKSRC)/cmdtocanonij2/filter/cmdtocanonij2: $(WRKSRC)/cmdtocanonij2/Makefile $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so
	$(MAKE) -C $(WRKSRC)/cmdtocanonij2
	$(MAKE) -C $(WRKSRC)/cnijbe2
	$(MAKE) -C $(WRKSRC)/lgmon3
	$(MAKE) -C $(WRKSRC)/rastertocanonij
	$(MAKE) -C $(WRKSRC)/tocanonij
	$(MAKE) -C $(WRKSRC)/tocnpwg

configure $(WRKSRC)/cmdtocanonij2/Makefile $(WRKSRC)/cnijbe2/Makefile $(WRKSRC)/lgmon3/Makefile $(WRKSRC)/rastertocanonij/Makefile $(WRKSRC)/tocanonij/Makefile $(WRKSRC)/tocnpwg/Makefile:
	cd $(WRKSRC)/cmdtocanonij2 && ./autogen.sh --prefix=$(PREFIX) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC)"
	cd $(WRKSRC)/cnijbe2 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) LDFLAGS="$(CUPS_LDFLAGS)"
	cd $(WRKSRC)/lgmon3 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC) $(CUPS_LDFLAGS)"
	cd $(WRKSRC)/rastertocanonij && ./autogen.sh --prefix=$(PREFIX) --enable-progpath=$(BINDIR)
	cd $(WRKSRC)/tocanonij && ./autogen.sh --prefix=$(PREFIX)
	cd $(WRKSRC)/tocnpwg && ./autogen.sh --prefix=$(PREFIX)

$(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so.5.0.0
	ln -svf libcnbpcnclapicom2.so.5.0.0 $@

$(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so.1.0.0
	ln -svf libcnbpnet30.so.1.0.0 $@

$(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so: $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so.1.2.4
	ln -svf libcnnet2.so.1.2.4 $@

test:
	LD_LIBRARY_PATH=/compat/linux/usr/lib64/:$(WRKSRC)/cnijfilter2/com/libs_bin64 cmdtocanonij2/filter/cmdtocanonij2

.PHONY: default build clean install test
