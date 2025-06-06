PORTNAME=	cnijfilter2
DISTVERSION=	5.40-1
CATEGORIES=	print
MASTER_SITES=	
DISTVERSIONPREFIX=	source-
ONLY_FOR_ARCHS=	i386 amd64

# DISTFILES=	cnijfilter2-source-${DISTVERSION}.tar.gz
# WRKSRC=	${WRKDIR}/cnijfilter2-source-${DISTVERSION}
#WRKSRC_SUBDIR=	cmdtocanonij2
#HAS_CONFIGURE=	yes
#GNU_CONFIGURE=	yes

EXTRA_PATCHES=	${FILESDIR}/config.patch
PATCH_STRIP= -p1

LIB_DEPENDS=	libcups.so:print/cups # /compat/linux/usr/lib/libdl.so.2:linux_base-rl9
RUN_DEPENDS=	${LIB_DEPENDS}

MAINTAINER=	ccpp@gmx.at
COMMENT=	Canon inkjet filters
WWW=	https://github.com/ccpp/freebsd-ports/

# FIXME: 32 if ${ARCH} = i386
ARC=64

#? 
#USE_LDCONFIG=	yes

# Fix finding cups
#USES=libtool
#USES=	pkgconfig
USES=	linux
USE_LINUX=	cups-libs	# HOW to? (from Epson)
#USE_LINUX_PREFIX

# pkg install linux-rl9-cups-libs

#CCUPS_LDFLAGS!=	pkg-config --libs-only-L cups
#CUPS_CFLAGS!=	pkg-config --cflags-only-I cups

do-configure:
	cd $(WRKSRC)/cmdtocanonij2 && ./autogen.sh --prefix=$(PREFIX) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC)"
	cd $(WRKSRC)/cnijbe2 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) LDFLAGS="$(CUPS_LDFLAGS)"
	cd $(WRKSRC)/lgmon3 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC) $(CUPS_LDFLAGS)"
	cd $(WRKSRC)/rastertocanonij && ./autogen.sh --prefix=$(PREFIX) --enable-progpath=$(BINDIR)
	cd $(WRKSRC)/tocanonij && ./autogen.sh --prefix=$(PREFIX)
	cd $(WRKSRC)/tocnpwg && ./autogen.sh --prefix=$(PREFIX)

do-build:
	${LN} -svf libcnbpcnclapicom2.so.5.0.0 $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so
	${LN} -svf libcnbpnet30.so.1.0.0 $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so
	${LN} -svf libcnnet2.so.1.2.4 $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so
	$(MAKE) -C $(WRKSRC)/cmdtocanonij2
	$(MAKE) -C $(WRKSRC)/cnijbe2
	$(MAKE) -C $(WRKSRC)/lgmon3
	$(MAKE) -C $(WRKSRC)/rastertocanonij
	$(MAKE) -C $(WRKSRC)/tocanonij
	$(MAKE) -C $(WRKSRC)/tocnpwg

do-install:
	$(MAKE) -C $(WRKSRC)/cmdtocanonij2 install
	$(MAKE) -C $(WRKSRC)/cnijbe2 install
	$(MAKE) -C $(WRKSRC)/lgmon3 install
	$(MAKE) -C $(WRKSRC)/rastertocanonij install
	$(MAKE) -C $(WRKSRC)/tocanonij install
	$(MAKE) -C $(WRKSRC)/tocnpwg install
	# TODO...

.include <bsd.port.mk>
