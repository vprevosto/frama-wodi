.include "../../mk/bsd.prefs.mk"
CONF_FRAMAC_API_DOC?=no
CONF_FRAMAC_ACSL_EL?=yes
CONF_FRAMAC_ENABLE_GUI?=auto
CONF_FRAMAC_START_SHORTCUT?=yes

VERSION=20120901
EXTRAVERSION=
PKGNAME=apps-frama-c-${VERSION}
PKGREVISION=
DISTNAME=frama-c-Oxygen-${VERSION}${EXTRAVERSION}
DISTFILES=${DISTNAME}.tar.gz
CATEGORIES=apps
MASTER_SITES=http://frama-c.com/download/
HOMEPAGE=http://frama-c.com/
COMMENT=Modular C static analyser
MAINTAINER=Virgile Prevosto <virgile.prevosto@m4x.org>

DEPENDS+=godi-ocaml>=3.10.2:../../godi/godi-ocaml

DEPENDS+=godi-ocamlgraph>=1.8:../../godi/godi-ocamlgraph

.if ${CONF_FRAMAC_ENABLE_GUI} == "yes"
CONFIGURE_ARGS+="--enable-gui"
DEPENDS+=godi-lablgtk2>=20090925:../../godi/godi-lablgtk2
.endif

.if ${CONF_FRAMAC_ENABLE_GUI} == "auto"
DEPENDS+=godi-lablgtk2>=20090925:../../godi/godi-lablgtk2
.endif

.if ${CONF_FRAMAC_ENABLE_GUI} == "no"
CONFIGURE_ARGS+="--disable-gui"
.endif

.if ${W32PORT} == "mingw"
FC_PREFIX=${LOCALBASE_NATIVE_BS}
.else
FC_PREFIX=${LOCALBASE}
.endif

CONFIGURE_ARGS += --prefix="${FC_PREFIX}"
CONFIGURE_ARGS += --with-cpp="$$(cygpath -m $$(LC_ALL=C /usr/sbin/update-alternatives --display gcc | head -n 2 | tail -n 1 | cut -d ' ' -f 6-)) -C -E -I."

USE_GMAKE= yes
HAS_CONFIGURE=yes
GNU_CONFIGURE=yes

.if ${CONF_FRAMAC_API_DOC} == "yes"
ALL_TARGET=all doc-distrib
.endif

.if ${PKGREVISION} != ""
post-extract:
	${MV} ${WRKSRC}/VERSION ${WRKSRC}/VERSION.old
	${SED} -e "s/\\(.*\\)$$/\\1-pl${PKGREVISION}/" ${WRKSRC}/VERSION.old > ${WRKSRC}/VERSION
.endif

pre-install:
.  if ${CONF_FRAMAC_ACSL_EL} == "yes"
	${MKDIR} ${PREFIX}/share/emacs/site-lisp
.  endif

SHORTCUT=${WRKDIR}/shortcut.vbs

post-install:
.  if ${CONF_FRAMAC_API_DOC} == "yes"
	${MKDIR} ${PREFIX}/share/frama-c/doc
	cd ${PREFIX}/share/frama-c/doc && ${GTAR} zxf ${WRKSRC}/frama-c-api.tar.gz
.  endif
	cp PLIST.common ${PLIST_SRC}
.  if ${W32PORT} == "mingw"
.  if ${CONF_FRAMAC_START_SHORTCUT} == "yes"
	${SED} \
        -e "s|@TARGET_PATH@|$$(cygpath -m ${PREFIX}/bin/frama-c-gui.exe)|" \
	-e \
       "s|@ICON_LOCATION@|$$(cygpath -m ${PREFIX}/share/frama-c/frama-c.ico)|" \
	-e "s|@TMP_FILE@|$$(cygpath -m ${WRKDIR}/lnk_loc.txt)|" \
        ${FILESDIR}/shortcut.vbs.in > ${SHORTCUT}
	cscript $$(cygpath -m ${SHORTCUT})
	echo "@unexec rm -f \"$$(cygpath -u -f ${WRKDIR}/lnk_loc.txt)\"" \
        >> ${PLIST_SRC}
.  endif
.  endif

.include "../../mk/bsd.pkg.mk"
