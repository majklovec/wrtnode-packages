include $(TOPDIR)/rules.mk

PKG_NAME:=node
PKG_VERSION:=0.10.33
PKG_RELEASE:=3

PKG_SOURCE:=node-v$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://nodejs.org/dist/v${PKG_VERSION}
PKG_MD5SUM:=626ca8a4f8fec4df49c78ed53d46f1f7
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-v$(PKG_VERSION)
GYP_DEFINES:=v8_use_mips_abi_hardfloat=false v8_can_use_fpu_instructions=false
LIBS:=-I$(TOOLCHAIN_DIR)/mipsel-openwrt-linux-uclibc/include/c++/4.8.3/ -I$(TOOLCHAIN_DIR)/mipsel-openwrt-linux-uclibc/include/c++/4.8.3/mipsel-openwrt-linux-uclibc/

include $(INCLUDE_DIR)/package.mk

define Package/node
  DEPENDS:=+libpthread +librt +uclibcxx
  SECTION:=lang
  SUBMENU:=Node.js
  CATEGORY:=Languages
  TITLE:=Node.js is a platform built on Chrome's JavaScript runtime
  URL:=http://nodejs.org/
endef

define Package/node/description
Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR); \
	export LIBS="$(LIBS)"; \
	export CFLAGS="$(TARGET_CFLAGS) $(LIBS)"; \
	export CXXFLAGS="$(TARGET_CXXFLAGS) $(LIBS)"; \
    export GYPFLAGS="$(GYPFLAGS)"; \
	./configure --dest-cpu=mipsel --dest-os=linux --without-snapshot --with-arm-float-abi=soft; \
	);
endef
#	./configure --dest-cpu=mipsel --dest-os=linux --without-ssl --without-snapshot --with-arm-float-abi=soft; \

define Build/Compile
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) GYP_DEFINES="$(GYP_DEFINES)" CC="$(TARGET_CC)" CXX="$(TARGET_CXX)" CFLAGS="$(TARGET_CFLAGS) $(LIBS)" CXXFLAGS="$(TARGET_CXXFLAGS) $(LIBS) -nostdinc++" LDFLAGS="$(TARGET_LDFLAGS) -nodefaultlibs -luClibc++ -lc -lgcc -lgcc_s -lpthread" || touch $(PKG_BUILD_DIR)/deps/v8/build/common.gypi
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) GYP_DEFINES="$(GYP_DEFINES)" CC="$(TARGET_CC)" CXX="$(TARGET_CXX)" CFLAGS="$(TARGET_CFLAGS) $(LIBS)" CXXFLAGS="$(TARGET_CXXFLAGS) $(LIBS) -nostdinc++" LDFLAGS="$(TARGET_LDFLAGS) -nodefaultlibs -luClibc++ -lc -lgcc -lgcc_s -lpthread"
endef


define Package/node/install
	mkdir -p $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/out/Release/node $(1)/usr/bin/
	ln -s /usr/bin/node $(1)/usr/bin/nodejs

	mkdir -p $(1)/usr/lib/node_modules
	$(CP) $(PKG_BUILD_DIR)/deps/npm $(1)/usr/lib/node_modules
	ln -sf /usr/lib/node_modules/npm/bin/npm-cli.js $(1)/usr/bin/npm
endef

$(eval $(call BuildPackage,node))
