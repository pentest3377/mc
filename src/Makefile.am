SUBDIRS = filemanager man2hlp vfs viewer

if USE_INTERNAL_EDIT
SUBDIRS += editor
endif

if USE_DIFF
SUBDIRS += diffviewer
endif

if ENABLE_SUBSHELL
SUBDIRS += subshell
endif

noinst_LTLIBRARIES = libinternal.la

AM_CPPFLAGS = \
	-DSYSCONFDIR=\""$(sysconfdir)/@PACKAGE@/"\" \
	-DLIBEXECDIR=\""$(libexecdir)/@PACKAGE@/"\" \
	-DDATADIR=\""$(pkgdatadir)/"\" \
	-DLOCALEDIR=\""$(localedir)"\" \
	-DEXTHELPERSDIR=\""@EXTHELPERSDIR@/"\"

if CONS_SAVER
SUBDIRS += consaver
AM_CPPFLAGS += -DSAVERDIR=\""$(pkglibexecdir)"\"
endif

# required for textconf.c
if ENABLE_VFS
if ENABLE_VFS_SFTP
AM_CPPFLAGS += $(LIBSSH_CFLAGS)
endif
endif

AM_CPPFLAGS += -I$(top_srcdir) $(GLIB_CFLAGS)

bin_PROGRAMS = mc

if USE_INTERNAL_EDIT
EDITLIB = editor/libedit.la
endif

if USE_DIFF
DIFFLIB = diffviewer/libdiffviewer.la
endif

if ENABLE_SUBSHELL
SUBSHELLLIB = subshell/libsubshell.la
endif

libinternal_la_LIBADD = \
	filemanager/libmcfilemanager.la \
	vfs/libmc-vfs.la \
	viewer/libmcviewer.la \
	$(DIFFLIB) $(EDITLIB) $(SUBSHELLLIB)

mc_LDADD = \
	libinternal.la

if ENABLE_MCLIB
libinternal_la_LIBADD += \
	$(top_builddir)/lib/libmc.la
else
mc_LDADD += \
	$(top_builddir)/lib/libmc.la
endif

SRC_mc_conssaver = \
	cons.handler.c consaver/cons.saver.h

mc_SOURCES = \
	main.c

libinternal_la_SOURCES = \
	$(SRC_mc_conssaver) \
	args.c args.h \
	clipboard.c clipboard.h \
	events_init.c events_init.h \
	execute.c execute.h \
	file_history.c file_history.h \
	help.c help.h \
	history.h \
	keymap.c keymap.h \
	learn.c learn.h \
	selcodepage.c selcodepage.h \
	setup.c setup.h \
	textconf.c textconf.h \
	usermenu.c usermenu.h \
	util.c util.h

if ENABLE_BACKGROUND
    libinternal_la_SOURCES += background.c background.h
endif

EXTRA_DIST = $(SRC_maintainer) $(SRC_charset)

# end of automated testing

install-exec-hook:
	$(MAKE) install_mcview
if USE_INTERNAL_EDIT
	$(MAKE) install_mcedit
endif
if USE_DIFF
	$(MAKE) install_mcdiff
endif

#
# Make relative symlinks, but do the right thing if LN_S is `ln' or `cp'.
#
install_mcview:
	cd $(DESTDIR)$(bindir)/$(binprefix) && rm -f mcview && $(LN_S) mc mcview

install_mcedit:
	cd $(DESTDIR)$(bindir)/$(binprefix) && rm -f mcedit && $(LN_S) mc mcedit

install_mcdiff:
	cd $(DESTDIR)$(bindir)/$(binprefix) && rm -f mcdiff && $(LN_S) mc mcdiff

uninstall-hook:
	rm -f $(DESTDIR)$(bindir)/$(binprefix)/mcview
if USE_INTERNAL_EDIT
	rm -f $(DESTDIR)$(bindir)/$(binprefix)/mcedit
endif
if USE_DIFF
	rm -f $(DESTDIR)$(bindir)/$(binprefix)/mcdiff
endif
