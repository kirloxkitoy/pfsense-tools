# New ports collection makefile for: dnscache
# Date created:		24 Jan 2000
# Whom:			Neil Blakey-Milner
#
# $FreeBSD: ports/dns/djbdns/Makefile,v 1.36 2009/09/04 14:09:27 roam Exp $
#

PORTNAME?=	djbdns
PORTVERSION?=	${DJBDNS_VER}
PORTREVISION?=	13
CATEGORIES?=	dns
MASTER_SITES=	http://cr.yp.to/djbdns/ \
		ftp://cr.yp.to/djbdns/
DISTNAME=	djbdns-${DJBDNS_VER}
DISTFILES=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER?=	roam@FreeBSD.org
COMMENT?=	A collection of secure and reliable DNS tools

RUN_DEPENDS=	setuidgid:${PORTSDIR}/sysutils/daemontools \
		tcpserver:${PORTSDIR}/sysutils/ucspi-tcp

OPTIONS=	DUMPCACHE	"persistent cache for dnscache" off \
		IPV6		"IPv6 support by Felix von Leitner" on \
		IGNOREIP	"ignoreip2 patch by Russell Nelson" off \
		JUMBO		"jumbo patch by Claudiu Costin" off \
		MAN		"manual pages by Gerritt Pape" on \
		PERSISTENT_MMAP	"persistent mmap patch by Lennert Buytenhek" off \
		SRV		"SRV record patch by Michael Handler" on

.include <bsd.port.pre.mk>

.if defined(WITH_JUMBO)
.if defined(WITH_IPV6) || defined(WITH_IGNOREIP) || defined(WITH_SRV)
BROKEN=		The jumbo patch currently conflicts with the IPv6, ignoreip2, and SRV patches
.endif

PATCH_SITES+=	http://people.FreeBSD.org/~roam/ports/dns/djbdns/:jumbo \
		${MASTER_SITE_LOCAL:S/%SUBDIR%\//roam\/dns\/djbdns\/:jumbo/}
PATCHFILES+=	jumbo-p13.patch.gz:jumbo
PATCH_DIST_STRIP=	-p1
.if defined(WITH_DUMPCACHE)
CFLAGS+=	-DDUMPCACHE
.endif
.endif

.if defined(WITH_MAN)
MANDATE=	-20031023
MASTER_SITES+=	http://smarden.org/pape/djb/manpages/:manpages
DISTFILES+=	${DISTNAME}-man${MANDATE}.tar.gz:manpages

MAN1=		dnsfilter.1 dnsip.1 dnsipq.1 dnsmx.1 dnsname.1 \
		dnsq.1 dnsqr.1 dnstrace.1 dnstracesort.1 dnstxt.1 \
		tinydns-get.1
MAN8=		axfr-get.8 axfrdns.8 axfrdns-conf.8 dnscache-conf.8 \
		dnscache.8 rbldns-conf.8 \
		rbldns-data.8 rbldns.8 tinydns-conf.8 tinydns-data.8 \
		tinydns-edit.8 tinydns.8 walldns-conf.8 walldns.8
MANCOMPRESSED=	no
.endif

.if defined(WITH_IPV6)
PKGNAMESUFFIX=	-ipv6
PORTVERSION=	${DJBDNS_VER}.${DJBDNS_V6_VER:S/test/b/:S/diff//}
PATCH_SITES+=	http://www.fefe.de/dns/:ipv6
PATCHFILES+=	${PORTNAME}-${DJBDNS_VER}-${DJBDNS_V6_VER}.diff.bz2:ipv6
PATCH_DIST_STRIP=	-p1
EXTRA_PATCHES+=	${FILESDIR}/uint32.patch ${FILESDIR}/djbdns-ipv6-ooyama.patch
.endif

.if defined(WITH_DUMPCACHE) && !defined(WITH_JUMBO)
.if defined(WITH_IPV6)
BROKEN=		The IPv6 and dnscache-dumpcache patches are currently in conflict
.endif
PATCH_SITES+=	http://efge.free.fr/djbdns/:dumpcache
PATCHFILES+=	patch-dnscache-dumpcache-v4.txt:dumpcache
PATCH_DIST_STRIP=	-p1
CFLAGS+=	-DDUMPCACHE
.endif

.if defined(WITH_IGNOREIP)
.if defined(WITH_IPV6)
BROKEN=		The IPv6 patch already includes the ignoreip2 functionality
.endif
PATCH_SITES+=	http://tinydns.org/:ignoreip
PATCHFILES+=	djbdns-1.05-ignoreip2.patch:ignoreip
PATCH_DIST_STRIP=	-p1
.endif

.if defined(WITH_PERSISTENT_MMAP)
PATCH_SITES+=	http://people.FreeBSD.org/~roam/ports/patches/dns/:persistent
PATCHFILES+=	tinydns-persistmmap-20040418.patch:persistent
PATCH_DIST_STRIP=	-p1
.endif

.if defined(WITH_SRV)
PATCH_SITES+=	http://tinydns.org/:srv
PATCHFILES+=	srv-patch:srv
PATCH_DIST_STRIP=	-p1
.endif

DJBDNS_VER=	1.05
DJBDNS_V6_VER=	test23

ALL_TARGET=	it
INSTALL_TARGET=	setup check

.if defined(WITH_IPV6)
PLIST_SUB+=	WITH_IPV6=""
.else
PLIST_SUB+=	WITH_IPV6="@comment "
.endif

post-patch:
	@echo "${CC} ${CFLAGS}" > ${WRKSRC}/conf-cc
	@echo "${CC} ${STRIP}" > ${WRKSRC}/conf-ld
	@echo "${PREFIX}" > ${WRKSRC}/conf-home

.if defined(WITH_MAN)
post-install:
	@${INSTALL_MAN} ${WRKDIR}/djbdns-man/*.1 ${PREFIX}/man/man1/
	@${INSTALL_MAN} ${WRKDIR}/djbdns-man/*.8 ${PREFIX}/man/man8/
.endif

.include <bsd.port.post.mk>
