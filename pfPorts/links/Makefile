# New ports collection makefile for:	links
# Date created:		21 January 2000
# Whom:			Michael Vasilenko <acid@stu.cn.ua>
#
# $FreeBSD: ports/www/links/Makefile,v 1.58 2009/07/31 13:56:29 dinoex Exp $
#

PORTNAME=	links
DISTVERSION=	2.2
PORTREVISION=	1
PORTEPOCH=	1
CATEGORIES?=	www
MASTER_SITES=	http://links.twibright.com/download/

PATCH_SITES=	ftp://ftp.linux.ee/pub/gentoo/distfiles/distfiles/ \
		ftp://ftp.osuosl.org/pub/blfs/conglomeration/links/ \
		ftp://ftp.ntua.gr/pub/linux/gentoo/distfiles/
PATCH_DIST_STRIP=-p1

MAINTAINER?=	demon@FreeBSD.org
COMMENT=	Lynx-like text WWW browser

CONFLICTS=	links-0*

GNU_CONFIGURE=	yes
CONFIGURE_ARGS+=	--with-ssl --without-pmshell --without-atheos \
			--without-gpm --without-fb
CONFIGURE_ENV+=	CPPFLAGS="${CPPFLAGS} -I${LOCALBASE}/include"
LDFLAGS+=	-L${LOCALBASE}/lib

USE_OPENSSL=	yes
USE_BZIP2=	yes

OPTIONS=	SVGALIB "non-X11 graphics support" off \
		DIRECTFB "directfb driver support" off \
		X11 "X11 graphics support" off \
		UTF8 "With gentoo utf-8 patch" on \
		TRANS "Hack for background transparency" off

.include <bsd.port.pre.mk>

.ifdef(WITH_SVGALIB) || defined(WITH_X11) || defined(WITH_DIRECTFB)
LIB_DEPENDS+=	png.5:${PORTSDIR}/graphics/png \
		jpeg.10:${PORTSDIR}/graphics/jpeg \
		tiff.4:${PORTSDIR}/graphics/tiff
CONFIGURE_ARGS+=	--enable-graphics
.else
CONFIGURE_ARGS+=	--disable-graphics --without-libjpeg --without-libtiff
.endif

.ifdef(WITH_SVGALIB)
LIB_DEPENDS+=	vga.1:${PORTSDIR}/graphics/svgalib
CONFIGURE_ARGS+=	--with-svgalib
.else
CONFIGURE_ARGS+=	--without-svgalib
.endif

.ifdef(WITH_DIRECTFB)
LIB_DEPENDS+=		directfb-1.1.1:${PORTSDIR}/devel/directfb
CONFIGURE_ARGS+=	--with-directfb
.else
CONFIGURE_ARGS+=	--without-directfb
.endif

.if !defined(WITHOUT_X11)
USE_XORG=	x11 xproto
CONFIGURE_ARGS+=	--with-x
.else
CONFIGURE_ARGS+=	--without-x
.endif

.if defined(WITH_UTF8)
PATCHFILES=	links-2.1pre33-utf8.diff.bz2
pre-configure:
		pwd && cd ${WRKSRC}/intl && ./gen-intl && ./synclang && cd ..
.endif

.if defined(WITH_TRANS)
EXTRA_PATCHES+=		${FILESDIR}/extra-terminal.c.diff
.endif

MAN1=		links.1
PLIST_FILES=	bin/links

post-patch:
	@${REINPLACE_CMD} -e "s!/etc/!${PREFIX}/etc/!" ${WRKSRC}/os_dep.h

.include <bsd.port.post.mk>
