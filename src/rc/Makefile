PROG=		rc
SRCS=		checkpath.c fstabinfo.c mountinfo.c rc.c rc-depend.c \
		rc-logger.c rc-misc.c rc-plugin.c rc-status.c rc-update.c \
		runscript.c start-stop-daemon.c

CLEANFILES=	version.h

LDFLAGS+=	-L../librc -L../libeinfo
LDADD+=		-lutil -lrc -leinfo
CFLAGS+=	-I../includes -I../librc -I../libeinfo

BINDIR?=	/sbin

LINKDIR=	${LIBNAME}/${PROG}
BINLINKS=	rc-status
SBINLINKS=	rc-update runscript start-stop-daemon
RC_BINLINKS=	einfon einfo ewarnn ewarn eerrorn eerror ebegin eend ewend \
		eindent eoutdent esyslog eval_ecolors \
		veinfo vewarn vebegin veend vewend veindent veoutdent \
		service_starting service_started \
		service_stopping service_stopped \
		service_inactive service_wasinactive \
		service_coldplugged service_started_daemon \
		checkpath fstabinfo mountinfo rc-depend \
		service_get_value service_set_value get_options save_options \
		shell_var
RC_SBINLINKS=	mark_service_starting mark_service_started \
		mark_service_stopping mark_service_stopped \
		mark_service_inactive mark_service_wasinactive \
		mark_service_coldplugged mark_service_failed \
		rc-abort

MK=		../../mk
include ${MK}/os.mk
CFLAGS+=	-DLIB=\"${LIBNAME}\"
include ${MK}/cc.mk
include ${MK}/prog.mk

include ${MK}/${TERMCAP}.mk
LDADD+=		${LIBDL} ${LIBKVM}
include ${MK}/${PAM}.mk

${SRCS}: version.h
version.h:
	sed -n -e 's/^VERSION=[[:space:]]*\([^[:space:]]*\).*/#define VERSION "\1\"/p' ../../Makefile > version.h
	if test -n "${BRANDING}"; then \
		echo "#define BRANDING \"${BRANDING}\"" >> version.h; \
	fi

install: all
	${INSTALL} -d ${DESTDIR}${BINDIR}
	${INSTALL} ${PROG} ${DESTDIR}${BINDIR}
	${INSTALL} -d ${DESTDIR}/bin
	for x in ${BINLINKS}; do ln -fs ${BINDIR}/${PROG} ${DESTDIR}/bin/$$x; done
	${INSTALL} -d ${DESTDIR}/bin
	for x in ${SBINLINKS}; do ln -fs ${PROG} ${DESTDIR}${BINDIR}/$$x; done
	${INSTALL} -d ${DESTDIR}/${LIBNAME}/${PROG}/bin
	for x in $(RC_BINLINKS); do ln -fs ${BINDIR}/${PROG} $(DESTDIR)/${LIBNAME}/${PROG}/bin/$$x; done
	${INSTALL} -d ${DESTDIR}/${LIBNAME}/${PROG}/sbin
	for x in ${RC_SBINLINKS}; do ln -fs ${BINDIR}/${PROG} ${DESTDIR}/${LIBNAME}/${PROG}/sbin/$$x; done
	if test "${PAM}" = pam; then \
		${INSTALL} -d ${DESTDIR}${PAMDIR}; \
		${INSTALL} -m ${PAMMODE} start-stop-daemon.pam ${DESTDIR}${PAMDIR}/start-stop-daemon; \
	fi