# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id$

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=kmod-switch
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/switch
  SUBMENU:=Other modules
  DEPENDS:=@TARGET_brcm_2_4||TARGET_brcm47xx
  TITLE:=Switch drivers
  FILES:= \
	$(PKG_BUILD_DIR)/switch-core.$(LINUX_KMOD_SUFFIX) \
	$(PKG_BUILD_DIR)/switch-adm.$(LINUX_KMOD_SUFFIX) \
	$(PKG_BUILD_DIR)/switch-robo.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoLoad,20,switch-core switch-robo switch-adm)
endef

define KernelPackage/switch/description
 This package contains switch drivers for ADM6996L and BCM53XX RoboSwitch.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

ifeq ($(BOARD),brcm-2.4)
  BUILDFLAGS := -DBROADCOM -DBCMDRIVER -I$(LINUX_DIR)/arch/mips/bcm947xx/include
else
  ifeq ($(BOARD),brcm47xx)
    BUILDFLAGS := -DBROADCOM
  endif
endif


define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		SUBDIRS="$(PKG_BUILD_DIR)" \
		EXTRA_CFLAGS="$(BUILDFLAGS)" \
		modules
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include
	$(CP) ./src/switch-core.h $(1)/usr/include/
endef

define Build/UninstallDev
	rm -f $(1)/usr/include/switch-core.h
endef

define KernelPackage/switch/install
	$(INSTALL_DIR) $(1)/lib/network/
	$(INSTALL_BIN) ./files/switch.sh $(1)/lib/network/
endef

$(eval $(call KernelPackage,switch))
