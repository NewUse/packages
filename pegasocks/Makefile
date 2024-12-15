#################################################
# OpenWrt C-based modern vpn proxy imlementation
#
#################################################

include $(TOPDIR)/rules.mk

# Name and release number of this package
PKG_NAME:=pegasocks
PKG_VERSION:=0.3
PKG_RELEASE:=15
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE_URL:=https://github.com/chux0519/pegasocks.git
PKG_SOURCE_BRANCH:=github/master
PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION).$(PKG_RELEASE)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)
PKG_SOURCE_VERSION:=8bf0a33ec25498f798bddade1e1425581d1afe6e
PKG_MIRROR_HASH:=014197d773cc96475d00648c095c3fc4762854d947fdee16cddd8e6cb6583290
PKG_HASH=ee46626b1821e34c026cf56ed6f20a1df945eb52fccc3f9bdc99590a1fc05572

PKG_CONFIG_DEPENDS:= \
	CONFIG_PEGASOCKS_GNUTLS \
	CONFIG_PEGASOCKS_OPENSSL \
	CONFIG_PEGASOCKS_MBEDTLS

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/pegasocks
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	TITLE:=pegasocks
	DEPENDS:= +PEGASOCKS_OPENSSL:libopenssl +PEGASOCKS_MBEDTLS:libmbedtls +PEGASOCKS_MBEDTLS:libevent2-mbedtls +libevent2-core +libevent2-extra +libevent2-pthreads +libpthread
	MENU:=1
endef

define Package/pegasocks/description
	OpenWrt C-based modern vpn proxy imlementation
endef

define Package/pegasocks/config
	source "$(SOURCE)/Config.in"
endef

CMAKE_OPTIONS += \
	-DCMAKE_BUILD_TYPE=MinSizeRel \
	$(if $(CONFIG_PEGASOCKS_OPENSSL),-DUSE_OPENSSL=on) \
	$(if $(CONFIG_PEGASOCKS_MBEDTLS),-DUSE_MBEDTLS=on) \
	$(if $(CONFIG_PEGASOCKS_GNUTLS),-DUSE_GNUTLS=on)

define Package/pegasocks/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpegas.a $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/pegas $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/pegasocks.config $(1)/etc/config/pegasocks
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/pegasocks.init $(1)/etc/init.d/pegasocks
	$(INSTALL_DIR) $(1)/etc/pegasocks
endef


$(eval $(call BuildPackage,$(PKG_NAME)))