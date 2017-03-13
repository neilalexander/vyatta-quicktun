#!/usr/bin/make -f

PACKAGE=vyatta-quicktun

DESTDIR=/opt/vyatta

TEMPLATECFGDIR=$DESTDIR/share/vyatta-cfg/template
TEMPLATEOPDIR=$DESTDIR/share/vyatta-op/template
SBINDIR=$DESTDIR/sbin/

BUILDDIR_QT=/tmp/quicktun

PREFIX='mipsel-linux-gnu-'

CFLAGS:=$(shell dpkg-buildflags --get CFLAGS)
LDFLAGS:=$(shell dpkg-buildflags --get LDFLAGS)

all: clean build package cleanpackage

build: quicktunclone quicktunbuild quicktunstage

package:
	tar czvf ../data.tar.gz opt
	cd debian && tar czvf ../../control.tar.gz *
	echo 2.0 > ../debian-binary
	ar -r ../$(PACKAGE).deb ../debian-binary ../control.tar.gz ../data.tar.gz

cleanpackage:
	rm ../debian-binary
	rm ../control.tar.gz
	rm ../data.tar.gz

quicktunclone:
	git clone https://github.com/neilalexander/quicktun $(BUILDDIR_QT)

quicktunbuild:
	cd $(BUILDDIR_QT) && cc="mipsel-linux-gnu-cc" ARCH="mipsel" LDFLAGS="-L./tmp/lib" ./build.sh

quicktunstage:
	cp $(BUILDDIR_QT)/out/quicktun opt/vyatta/sbin/quicktun
	chmod +x opt/vyatta/sbin/quicktun

clean:
	rm -rf opt/vyatta/sbin/quicktun
	rm -rf $(BUILDDIR_QT)

distclean:	clean
