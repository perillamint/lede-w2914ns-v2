#
# Copyright (C) 2016 Jiang Yutang <jiangyutang1978@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

PKG_NAME:=uboot-layerscape-32bit
PKG_VERSION:=v1.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/fsl-jyt/uboot-ls-32b.git
PKG_SOURCE_VERSION:=719f8b606334bc73367f78c204dce69786963b65

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

PKG_LICENSE:=GPL-2.0 GPL-2.0+
PKG_LICENSE_FILES:=Licenses/README

include $(INCLUDE_DIR)/package.mk

define uboot/Default
	TITLE:=
	CONFIG:=
endef

define uboot/ls1043ardb
	TITLE:=U-Boot binary $(PKG_NAME)-$(PKG_VERSION) for NXP ls1043ardb 32b Dev Board
	CONFIG=ls1043ardb-uboot.bin
endef

define uboot/ls1012ardb
	TITLE:=U-Boot binary $(PKG_NAME)-$(PKG_VERSION) for NXP ls1012ardb 32b Dev Board
	CONFIG=ls1012ardb-uboot.bin.swap
endef

UBOOTS := \
	ls1043ardb \
	ls1012ardb

define Package/uboot/template
define Package/uboot-layerscape-32b-$(1)
	SECTION:=boot
	CATEGORY:=Boot Loaders
	DEPENDS:= @TARGET_layerscape_32b
	TITLE:=$(2)
	URL:=https://github.com/fsl-jyt/uboot-ls-32b.git
	VARIANT:=$(1)
	MAINTAINER:=Jiang Yutang <jiangyutang1978@gmail.com>
endef
endef

define BuildUBootPackage
	$(eval $(uboot/Default))
	$(eval $(uboot/$(1)))
	$(call Package/uboot/template,$(1),$(TITLE))
endef

ifdef BUILD_VARIANT
$(eval $(call uboot/$(BUILD_VARIANT)))
UBOOT_CONFIG:=$(if $(CONFIG),$(CONFIG),$(BUILD_VARIANT))
endif

define Build/Configure
endef

define Build/Compile
endef

define Package/uboot/install/default
	$(CP) $(PKG_BUILD_DIR)/$(UBOOT_CONFIG) $(KDIR)/$(1)-32b-uboot.bin
endef

define Package/uboot/install/template
define Package/uboot-layerscape-32b-$(1)/install
	$(call Package/uboot/install/default,$(2))
endef
endef

$(foreach u,$(UBOOTS), \
	$(eval $(call Package/uboot/install/template,$(u),$(u))) \
)

$(foreach u,$(UBOOTS), \
	$(eval $(call BuildUBootPackage,$(u))) \
	$(eval $(call BuildPackage,uboot-layerscape-32b-$(u))) \
)
