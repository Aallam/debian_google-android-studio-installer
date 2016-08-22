PKG_NAME:=android-studio

PKG_SOURCE:=${PKG_NAME}-ide-${PKG_VERSION}-linux.zip
PKG_SOURCE_URL:=${DL_MIRROR}${PKG_SOURCE}

DL_DIR=/var/cache/google-${PKG_NAME}-installer
UNPACK_DIR=$(DL_DIR)/android-studio
INSTALL_DIR=/usr/lib/${PKG_NAME}
DOC_DIR=/usr/share/doc/google-${PKG_NAME}

all: $(UNPACK_DIR)/LICENSE.txt

install: all
	install -d -m0755 $(DOC_DIR)
	install -m0644 $(UNPACK_DIR)/LICENSE.txt $(DOC_DIR)/copyright
	gzip -9 --stdout $(UNPACK_DIR)/NOTICE.txt > $(DOC_DIR)/copyright.gz
	find ${DOC_DIR} | sort >> /var/lib/dpkg/info/google-${PKG_NAME}-installer.list
	@if [ ! -d $(INSTALL_DIR) ]; then \
		mv $(UNPACK_DIR) $(INSTALL_DIR) ; \
		find ${INSTALL_DIR} | sort >> /var/lib/dpkg/info/google-${PKG_NAME}-installer.list ;\
	else\
		echo "\033[0;31m$(INSTALL_DIR) already exists.\033[0m" ; \
		exit 1 ; \
	fi

$(UNPACK_DIR)/LICENSE.txt: $(DL_DIR)/$(PKG_SOURCE)
	cd $(DL_DIR) && unzip -ou $(PKG_SOURCE)

$(DL_DIR)/$(PKG_SOURCE):
	cd $(DL_DIR) && \
		su nobody -s /bin/sh -c "wget --continue $(PKG_SOURCE_URL)"
	sha1sum -c $(PKG_SOURCE).sha1

clean:
	-rm -rf -- $(UNPACK_DIR)

distclean: clean
	-rm -rf -- $(DL_DIR)

.PHONY: install clean
