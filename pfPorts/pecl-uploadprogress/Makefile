# New ports collection makefile for:	pecl-uploadprogress
# Date created:			2006/12/06
# Whom:				chinsan
#
# $FreeBSD: ports/devel/pecl-uploadprogress/Makefile,v 1.3 2008/08/29 17:47:36 chinsan Exp $
#

PORTNAME=	uploadprogress
PORTVERSION=	0.9.1
CATEGORIES=	devel pear
MASTER_SITES=	http://pecl.php.net/get/
PKGNAMEPREFIX=	pecl-
EXTRACT_SUFX=	.tgz
DIST_SUBDIR=	PECL

MAINTAINER=	chinsan@FreeBSD.org
COMMENT=	Provides track progress of a file upload

USE_PHP=	yes
USE_PHPEXT=	yes
IGNORE_WITH_PHP=4

CONFIGURE_ARGS=	--enable-uploadprogress

.include <bsd.port.mk>
