# New ports collection makefile for:		Binary patch tool
# Date created:					7 June 2003
# Whom:						cperciva@daemonology.net
#
# $FreeBSD: ports/misc/bsdiff/Makefile,v 1.8 2006/09/13 05:24:33 cperciva Exp $
#

PORTNAME=	bsdiff
PORTVERSION=	4.3
CATEGORIES=	misc devel
MASTER_SITES=	http://www.daemonology.net/bsdiff/

MAINTAINER=	cperciva@daemonology.net
COMMENT=	Generates and applies patches to binary files

MAN1=		bsdiff.1 bspatch.1
PLIST_FILES=	bin/bsdiff bin/bspatch

.include <bsd.port.pre.mk>

.if ${OSVERSION} > 600033 || ( ${OSVERSION} > 504103 && ${OSVERSION} < 600000 )
IGNORE=		is now contained in the base system
.endif

.include <bsd.port.post.mk>
