#
# Copyright (C) 2010-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=usbip
PKG_VERSION:=0.1.7
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/usbip
PKG_MD5SUM:=d1094b6d4449787864f8be001639232c

PKG_BUILD_DEPENDS:=glib2 sysfsutils
PKG_FIXUP:=libtool autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/usbip/Common
  TITLE:=USB-over-IP
  URL:=http://usbip.sourceforge.net/
  DEPENDS:=@!LINUX_2_4 @USB_SUPPORT
  MAINTAINER:=Nuno Goncalves <nunojpg@gmail.com>
endef

define Package/usbip/Default
$(call Package/usbip/Common)
  SECTION:=net
  CATEGORY:=Network
endef

define Package/usbip
$(call Package/usbip/Default)
  TITLE+= (common)
  DEPENDS+= +libsysfs +libwrap +kmod-usbip
endef

define Package/usbip-client
$(call Package/usbip/Default)
  TITLE+= (client)
  DEPENDS+= usbip +glib2 +kmod-usbip-client
endef

define Package/usbip-server
$(call Package/usbip/Default)
  TITLE+= (server)
  DEPENDS+= usbip +glib2 +kmod-usbip-server
endef

define KernelPackage/usbip/Default
$(call Package/usbip/Common)
  SUBMENU:=USB Support
endef

define KernelPackage/usbip
$(call KernelPackage/usbip/Default)
  TITLE+= (kernel support)
  DEPENDS+= +kmod-usb-core
  KCONFIG:= \
	CONFIG_USB_IP_COMMON CONFIG_USB_IP_DEBUG_ENABLE=n \
	CONFIG_USBIP_CORE CONFIG_USBIP_DEBUG=n
 ifeq ($(strip $(call CompareKernelPatchVer,$(KERNEL_PATCHVER),le,2.6.39)),1)
  FILES:=$(LINUX_DIR)/drivers/staging/usbip/usbip_common_mod.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoLoad,98,usbip_common_mod)
 else
  FILES:=$(LINUX_DIR)/drivers/staging/usbip/usbip-core.ko
  AUTOLOAD:=$(call AutoLoad,98,usbip-core)
 endif
endef

define KernelPackage/usbip-client
$(call KernelPackage/usbip/Default)
  TITLE+= (kernel client driver)
  DEPENDS+= kmod-usbip
  KCONFIG:= CONFIG_USB_IP_VHCI_HCD CONFIG_USBIP_VHCI_HCD
  FILES:=$(LINUX_DIR)/drivers/staging/usbip/vhci-hcd.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoLoad,99,vhci-hcd)
endef

define KernelPackage/usbip-server
$(call KernelPackage/usbip/Default)
  TITLE+= (kernel host driver)
  DEPENDS+= kmod-usbip
  KCONFIG:= CONFIG_USB_IP_HOST CONFIG_USBIP_HOST
 ifeq ($(strip $(call CompareKernelPatchVer,$(KERNEL_PATCHVER),le,2.6.39)),1)
  FILES:=$(LINUX_DIR)/drivers/staging/usbip/usbip.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoLoad,99,usbip)
 else
  FILES:=$(LINUX_DIR)/drivers/staging/usbip/usbip-host.ko
  AUTOLOAD:=$(call AutoLoad,99,usbip-host)
 endif
endef

include $(INCLUDE_DIR)/kernel-defaults.mk

CONFIGURE_PATH:=./src
MAKE_PATH:=./src
LIBTOOL_PATHS:=./src

CFLAGS+="$(TARGET_CFLAGS) -I$(STAGING_DIR)/usr/include"

define Build/Compile/kmod
	$(MAKE) $(KERNEL_MAKEOPTS) \
		SUBDIRS="$(LINUX_DIR)/drivers/staging/usbip" \
		CONFIG_USB_IP_COMMON=m \
		CONFIG_USB_IP_VHCI_HCD=m \
		CONFIG_USB_IP_HOST=m \
		CONFIG_USBIP_CORE=m \
		CONFIG_USBIP_VHCI_HCD=m \
		CONFIG_USBIP_HOST=m \
		modules
endef

define Build/Compile
$(call Build/Compile/Default)
$(call Build/Compile/kmod)
endef

define Package/usbip/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libusbip.so.* \
		$(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/share/usbip
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/share/usbip/usb.ids \
		$(1)/usr/share/usbip/
endef

define Package/usbip-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/bin/usbip \
		$(1)/usr/bin/
endef

define Package/usbip-server/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/bin/usbipd \
		$(1)/usr/bin/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/bin/bind_driver \
		$(1)/usr/bin/usbip_bind_driver
endef

$(eval $(call BuildPackage,usbip))
$(eval $(call BuildPackage,usbip-client))
$(eval $(call BuildPackage,usbip-server))
$(eval $(call KernelPackage,usbip))
$(eval $(call KernelPackage,usbip-client))
$(eval $(call KernelPackage,usbip-server))
