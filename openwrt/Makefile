include $(TOPDIR)/rules.mk

PKG_NAME:=speedtest-cpp
PKG_VERSION:=2.0.0
PKG_RELEASE:=1

PKG_MAINTAINER:=Vad <vad@example.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

# When building from a local checkout (e.g. CI), set USE_SOURCE_DIR to skip
# the tarball download. The line below is replaced by the build workflow.
# USE_SOURCE_DIR_PLACEHOLDER

include $(INCLUDE_DIR)/package.mk

define Package/libspeedtestcpp
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=speedtest.net client library (C++)
  URL:=https://github.com/vad-b/speedtestcpp
  DEPENDS:=+libcurl +libstdcpp
endef

define Package/libspeedtestcpp/description
  Shared library backing the speedtest-cpp CLI: C++ implementation of a
  speedtest.net client using the raw TCP protocol for better accuracy.
endef

define Package/speedtest-cpp
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Command line speedtest.net client (C++)
  URL:=https://github.com/vad-b/speedtestcpp
  DEPENDS:=+libcurl +libstdcpp +libspeedtestcpp
endef

define Package/speedtest-cpp/description
  Unofficial command line interface for testing internet connection
  bandwidth using speedtest.net.
endef

MAKE_FLAGS += \
	CXX="$(TARGET_CXX)" \
	AR="$(TARGET_AR)" \
	LN="ln" \
	CXXFLAGS="$(TARGET_CXXFLAGS) -std=c++23 -Wall -fPIC" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	SPEEDTEST_LINK_SHARED=yes

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_FLAGS) speedtest
endef

define Package/libspeedtestcpp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libspeedtestcpp.so.2.0.0 $(1)/usr/lib/
	ln -sf libspeedtestcpp.so.2.0.0 $(1)/usr/lib/libspeedtestcpp.so
endef

define Package/speedtest-cpp/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/speedtest $(1)/usr/bin/speedtest-cpp
endef

$(eval $(call BuildPackage,libspeedtestcpp))
$(eval $(call BuildPackage,speedtest-cpp))
