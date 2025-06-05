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

LIB_DEPENDS=	libcups.so:print/cups libdl.so.2:linux_base-rl9
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
USES=	pkgconfig

#CCUPS_LDFLAGS!=	pkg-config --libs-only-L cups
#CUPS_CFLAGS!=	pkg-config --cflags-only-I cups

# Allow to forward our CFLAGS
pre-configure:
	#sed -i -e 's/^CFLAGS = /CFLAGS = $${CFLAGS} /' ${WRKSRC}/lgmon3/src/Makefile.am
	#sed -i -e 's/^CFLAGS="/CFLAGS="<$${CFLAGS}> /' ${WRKSRC}/lgmon3/configure.in
	#sed -i -e 's/^CFLAGS =/CFLAGS +=/' ${WRKSRC}/lgmon3/src/Makefile.am
	sed -i -e 's/^CFLAGS=.*/PKG_CHECK_MODULES([CUPS], [cups])/' ${WRKSRC}/lgmon3/configure.in
	sed -i -e 's/^XML2_CFLAGS=.*/PKG_CHECK_MODULES([XML2], [libxml-2.0])/' ${WRKSRC}/lgmon3/configure.in
	sed -i -e 's/AC_SUBST.XML2_CFLAGS.//' ${WRKSRC}/lgmon3/configure.in
	sed -i -e 's/-lusb-1.0/-lusb/' ${WRKSRC}/lgmon3/src/Makefile.am
	sed -i -e 's/@XML2_CFLAGS@/-lintl @XML2_CFLAGS@ @CUPS_CFLAGS@\nLDFLAGS = @XML2_LIBS@/' ${WRKSRC}/lgmon3/src/Makefile.am
	sed -i -e 's/^int.*GET_PROTOCOL.*;$$/static &/' ${WRKSRC}/lgmon3/src/*.c

do-configure:
	cd $(WRKSRC)/cmdtocanonij2 && ./autogen.sh --prefix=$(PREFIX) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC)"
	cd $(WRKSRC)/cnijbe2 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) LDFLAGS="$(CUPS_LDFLAGS)"
	cd $(WRKSRC)/lgmon3 && ./autogen.sh --prefix=$(PREFIX) --enable-libpath=$(LIBDIR) --enable-progpath=$(BINDIR) --datadir=$(DATADIR) LDFLAGS="-L../../com/libs_bin$(ARC) $(CUPS_LDFLAGS)"
	cd $(WRKSRC)/rastertocanonij && ./autogen.sh --prefix=$(PREFIX) --enable-progpath=$(BINDIR)
	cd $(WRKSRC)/tocanonij && ./autogen.sh --prefix=$(PREFIX)
	cd $(WRKSRC)/tocnpwg && ./autogen.sh --prefix=$(PREFIX)

do-build:
	ln -sv libcnbpcnclapicom2.so.5.0.0 $(WRKSRC)/com/libs_bin$(ARC)/libcnbpcnclapicom2.so
	ln -sv libcnbpnet30.so.1.0.0 $(WRKSRC)/com/libs_bin$(ARC)/libcnbpnet30.so
	ln -sv libcnnet2.so.1.2.4 $(WRKSRC)/com/libs_bin$(ARC)/libcnnet2.so
	$(MAKE) -C $(WRKSRC)/cmdtocanonij2
	$(MAKE) -C $(WRKSRC)/cnijbe2
	$(MAKE) -C $(WRKSRC)/lgmon3
	$(MAKE) -C $(WRKSRC)/rastertocanonij
	$(MAKE) -C $(WRKSRC)/tocanonij
	$(MAKE) -C $(WRKSRC)/tocnpwg


.include <bsd.port.mk>
