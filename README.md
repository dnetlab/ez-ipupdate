# ez-ipupdate

a copy from https://sourceforge.net/projects/ez-ipupdate/files/ez-ipupdate/

## Makefile For Openwrt
```Makefile
#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ezipupdate
PKG_VERSION:=3.0.10
PKG_RELEASE:=$(PKG_SOURCE_VERSION)

PKG_LICENSE:=GPL-2.0

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/dnetlab/ez-ipupdate.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=999bca5262c6217da5121bd62205d267801f434c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Dnetlab
  SUBMENU:=ddns
  TITLE:=Ez-ipupdate is a client for the dynamic IP service
  URL:=http://www.ez-ip.net
  DEPENDS:=+ddns
endef

define Package/$(PKG_NAME)/description
 It has several options and is quite complete. It is written in pure 
 C and supports a daemon mode (Linux only right now).This program has 
 been tested under Linux and Solaris.
endef

CONFIGURE_ARGS+= \
	--enable-shared

define Build/Compile/$(PKG_NAME)
	$(call Build/Configure/Default,,,$(PKG_NAME)) 
	$(MAKE) -C $(PKG_BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION) \
		$(TARGET_CONFIGURE_OPTS) \
		CFLAGS="$(TARGET_CFLAGS)" \
		CPPFLAGS="$(TARGET_CPPFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ez-ipupdate $(1)/usr/sbin/
endef
# $(INSTALL_BIN) $(PKG_BUILD_DIR)/ez-ipupdate/ez-ipupdate $(1)/usr/sbin/

$(eval $(call BuildPackage,$(PKG_NAME)))
```