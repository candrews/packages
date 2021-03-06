#
# Copyright (C) 2009-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/uclibc++.mk

BASE_VERSION:=4.7.25

PKG_NAME:=db47
PKG_VERSION:=$(BASE_VERSION).4.NC
PKG_RELEASE:=2

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/db-$(BASE_VERSION).NC
PKG_SOURCE:=db-$(BASE_VERSION).NC.tar.gz
PKG_SOURCE_URL:=http://download.oracle.com/berkeley-db/
PKG_MD5SUM:=073ab7f20d24b3872a51ca762f5090e7

PKG_MAINTAINER:=Marcel Denia <naoir@gmx.net>
PKG_LICENSE:=Sleepycat
PKG_LICENSE_FILES:=LICENSE

PKG_FIXUP:=autoreconf
PKG_LIBTOOL_PATHS:=. build_unix
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libdb47/Default
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libxml2
  PROVIDES:=libdb47
  URL:=http://www.oracle.com/us/products/database/berkeley-db
endef

define Package/libdb47/Default/description
  Berkeley DB library (4.7).
endef

define Package/libdb47
$(call Package/libdb47/Default)
  VARIANT:=small
  TITLE:=Berkeley DB library (4.7) (without statistics etc. support)
endef

define Package/libdb47/description
$(call Package/libdb47/Default/description)
 This package is not built with statistics etc. support.
endef

define Package/libdb47-full
$(call Package/libdb47/Default)
  VARIANT:=full
  TITLE:=Berkeley DB library (4.7) (with statistics etc. support)
endef

define Package/libdb47-full/description
$(call Package/libdb47/Default/description)
 This package is built with statistics etc. support.
endef

define Package/libdb47xx/Default
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libdb47 $(CXX_DEPENDS)
  URL:=http://www.oracle.com/us/products/database/berkeley-db
endef

define Package/libdb47xx/Default/description
  Berkeley DB library (4.7).  C++ wrapper.
endef

define Package/libdb47xx
$(call Package/libdb47xx/Default)
  VARIANT:=small
  TITLE:=Berkeley DB library (4.7) for C++ (without statistics etc. support)
endef

define Package/libdb47xx/description
$(call Package/libdb47xx/Default/description)
 This package is not built with statistics etc. support.
endef

define Package/libdb47xx-full
$(call Package/libdb47xx/Default)
  VARIANT:=full
  TITLE:=Berkeley DB library (4.7) for C++ (with statistics etc. support)
endef

define Package/libdb47xx-full/description
$(call Package/libdb47xx/Default/description)
 This package is built with statistics etc. support.
endef

CONFIGURE_PATH = build_unix
CONFIGURE_CMD = ../dist/configure

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-java \
	--with-mutex=UNIX/fcntl \
	--disable-tcl \
	--disable-rpc \
	--enable-compat185 \
	--disable-debug \
	--enable-cryptography \
	$(if $(CONFIG_PACKAGE_libdb47xx),--enable-cxx,--disable-cxx)

ifeq ($(BUILD_VARIANT),small)
        CONFIGURE_ARGS += --enable-smallbuild
endif

TARGET_CFLAGS += $(FPIC)

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/build_unix \
		DESTDIR="$(PKG_INSTALL_DIR)" all
	$(MAKE) -C $(PKG_BUILD_DIR)/build_unix \
		DESTDIR="$(PKG_INSTALL_DIR)" install
endef

define Package/libdb47/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb-*.so $(1)/usr/lib/
endef

define Package/libdb47-full/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb-*.so $(1)/usr/lib/
endef

define Package/libdb47xx/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb_cxx-*.so $(1)/usr/lib/
endef

define Package/libdb47xx-full/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb_cxx-*.so $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/db.h $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/db_cxx.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb*.{a,so} $(1)/usr/lib
endef

$(eval $(call BuildPackage,libdb47))
$(eval $(call BuildPackage,libdb47-full))
$(eval $(call BuildPackage,libdb47xx))
$(eval $(call BuildPackage,libdb47xx-full))
