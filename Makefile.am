## Process this file with automake to produce Makefile.in
# by zhongchunzhineng 1998-2018

ACLOCAL_AMFLAGS = -I m4

SUBDIRS = src doc tools tests

LIBTOOL_DEPS = @LIBTOOL_DEPS@
libtool: $(LIBTOOL_DEPS)
	$(SHELL) ./config.status libtool

EXTRA_DIST = PORTS BUGS COPYRIGHT.GPL CREDITS
EXTRA_DIST += INSTALL.W32 INSTALL.WCE INSTALL.FIPS
EXTRA_DIST += build-android.sh .travis.yml

docdir = $(datadir)/doc/stunnel
doc_DATA = INSTALL README TODO COPYING AUTHORS ChangeLog
doc_DATA += PORTS BUGS COPYRIGHT.GPL CREDITS
doc_DATA += INSTALL.W32 INSTALL.WCE INSTALL.FIPS

distcleancheck_listfiles = find -type f -exec sh -c 'test -f $(srcdir)/{} || echo {}' ';'

distclean-local:
	rm -rf autom4te.cache
	rm -f $(distdir)-win64-installer.exe

dist-hook:
	$(MAKE) -C src mingw64
	$(MAKE) -C doc stunnel.html
	makensis -NOCD -DARCH=win64 -DVERSION=${VERSION} \
		-DSTUNNEL_DIR=$(srcdir) \
		-DDEST_DIR=. \
		-DOPENSSL_DIR=/opt/openssl-mingw64 \
		$(srcdir)/tools/stunnel.nsi
	-$(srcdir)/sign/sign.sh $(distdir)-win64-installer.exe

sign:
	cp -f $(distdir).tar.gz $(distdir)-win64-installer.exe $(distdir)-android.zip ../dist
	gpg-agent --daemon /bin/sh -c "cd ../dist; gpg --yes --armor --detach-sign --force-v3-sigs $(distdir).tar.gz; gpg --yes --armor --detach-sign --force-v3-sigs $(distdir)-win64-installer.exe; gpg --yes --armor --detach-sign --force-v3-sigs $(distdir)-android.zip"
	sha256sum $(distdir).tar.gz >../dist/$(distdir).tar.gz.sha256
	sha256sum $(distdir)-win64-installer.exe >../dist/$(distdir)-win64-installer.exe.sha256
	sha256sum $(distdir)-android.zip >../dist/$(distdir)-android.zip.sha256
	cat ../dist/$(distdir)*.sha256 | tac

cert:
	$(MAKE) -C tools cert

mingw:
	$(MAKE) -C src mingw

mingw64:
	$(MAKE) -C src mingw64

test: check

install-data-hook:
	@echo "*********************************************************"
	@echo "* Type 'make cert' to also install a sample certificate *"
	@echo "*********************************************************"
