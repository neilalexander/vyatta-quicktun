#!/usr/bin/make -f

BUILDDIR_QT=/tmp/quicktun

PREFIX=mipsel-linux-gnu-

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
	mkdir $(BUILDDIR_QT)/obj $(BUILDDIR_QT)/tmp $(BUILDDIR_QT)/out $(BUILDDIR_QT)/tmp/include $(BUILDDIR_QT)/tmp/lib
	echo '#include "$(BUILDDIR_QT)/src/tweetnacl.h"' > $(BUILDDIR_QT)/tmp/include/crypto_box_curve25519xsalsa20poly1305.h
	echo '#include "$(BUILDDIR_QT)/src/tweetnacl.h"' > $(BUILDDIR_QT)/tmp/include/crypto_scalarmult_curve25519.h
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/tweetnacl.c     -o obj/tweetnacl.o     -I./tmp/include/
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/randombytes.c   -o obj/randombytes.o   -I./tmp/include/
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/proto.raw.c     -o obj/proto.raw.o     -I./tmp/include/
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/proto.nacl0.c   -o obj/proto.nacl0.o   -I./tmp/include/
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/proto.nacltai.c -o obj/proto.nacltai.o -I./tmp/include/
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/proto.salty.c   -o obj/proto.salty.o   -I./tmp/include/
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c -DCOMBINED_BINARY src/run.combined.c  -o obj/run.combined.o
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(CFLAGS) -c                   src/common.c        -o obj/common.o        -DQT_VERSION=\"`cat version`\"
	cd $(BUILDDIR_QT) && $(PREFIX)cc $(LDFLAGS) -o out/quicktun obj/*.o

quicktunstage:
	cp $(BUILDDIR_QT)/out/quicktun opt/vyatta/sbin/quicktun
	chmod +x opt/vyatta/sbin/quicktun

clean:
	rm -rf opt/vyatta/sbin/quicktun
	rm -rf $(BUILDDIR_QT)

distclean:	clean
