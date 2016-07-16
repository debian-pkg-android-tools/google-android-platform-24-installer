PKG_NAME:=platform-24
PKG_VERSION:=01

PKG_SOURCE:=${PKG_NAME}_r${PKG_VERSION}.zip
PKG_SOURCE_URL:=${DL_MIRROR}/android/repository/${PKG_SOURCE}

DL_DIR=/var/cache/google-android-${PKG_NAME}-installer
UNPACK_DIR=$(DL_DIR)/android-N
INSTALL_DIR=/usr/lib/android-sdk/platforms
DOC_DIR=/usr/share/doc/google-android-${PKG_NAME}

all: $(UNPACK_DIR)/source.properties

install: all
	install -d -m0755 $(DOC_DIR)
	gzip -9 --stdout $(UNPACK_DIR)/data/NOTICE.txt > $(DOC_DIR)/copyright.gz
	install -m0644 $(UNPACK_DIR)/source.properties $(DOC_DIR)/
	find ${DOC_DIR} | sort >> /var/lib/dpkg/info/google-android-${PKG_NAME}-installer.list
	install -d -m0755 $(INSTALL_DIR)
	@if [ ! -d $(INSTALL_DIR)/android-24 ]; then \
		mv $(UNPACK_DIR) $(INSTALL_DIR)/android-24 ;\
		find ${INSTALL_DIR}/android-24 | sort >> /var/lib/dpkg/info/google-android-${PKG_NAME}-installer.list ;\
	else\
		echo "$(INSTALL_DIR)/android-24 already exists, nothing copied." ; \
	fi

$(UNPACK_DIR)/source.properties: $(DL_DIR)/$(PKG_SOURCE)
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
