#
# Copyright (C) 2018-2023 Jeffery To
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

GO_VERSION_MAJOR_MINOR:=1.23
GO_VERSION_PATCH:=11

PKG_NAME:=golang
PKG_VERSION:=$(GO_VERSION_MAJOR_MINOR)$(if $(GO_VERSION_PATCH),.$(GO_VERSION_PATCH))
PKG_RELEASE:=1

GO_SOURCE_URLS:=https://dl.google.com/go/ \
                https://mirrors.ustc.edu.cn/golang/ \
                https://mirrors.nju.edu.cn/golang/

PKG_SOURCE:=go$(PKG_VERSION).src.tar.gz
PKG_SOURCE_URL:=$(GO_SOURCE_URLS)
PKG_HASH:=296381607a483a8a8667d7695331752f94a1f231c204e2527d2f22e1e3d1247d

PKG_MAINTAINER:=Jeffery To <jeffery.to@gmail.com>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_CPE_ID:=cpe:/a:golang:go

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_DIR:=$(BUILD_DIR)/go-$(PKG_VERSION)
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

PKG_GO_PREFIX:=/usr
PKG_GO_VERSION_ID:=$(GO_VERSION_MAJOR_MINOR)

HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/go-$(PKG_VERSION)
HOST_BUILD_PARALLEL:=1

HOST_GO_PREFIX:=$(STAGING_DIR_HOSTPKG)
HOST_GO_VERSION_ID:=cross
HOST_GO_ROOT:=$(HOST_GO_PREFIX)/lib/go-$(HOST_GO_VERSION_ID)

HOST_GO_VALID_OS_ARCH:= \
  android_386  android_amd64  android_arm  android_arm64 \
  freebsd_386  freebsd_amd64  freebsd_arm  freebsd_arm64 \
  linux_386    linux_amd64    linux_arm    linux_arm64 \
  openbsd_386  openbsd_amd64  openbsd_arm  openbsd_arm64 \
  netbsd_386   netbsd_amd64   netbsd_arm   netbsd_arm64 \
  windows_386  windows_amd64  windows_arm  windows_arm64 \
  \
  plan9_386    plan9_amd64    plan9_arm \
  \
  darwin_amd64 darwin_arm64 \
  ios_amd64    ios_arm64 \
  \
  dragonfly_amd64 \
  illumos_amd64 \
  solaris_amd64 \
  \
  aix_ppc64 \
  js_wasm \
  wasip1_wasm \
  \
  freebsd_riscv64 \
  openbsd_riscv64 \
  \
  linux_ppc64 linux_ppc64le \
  linux_mips linux_mipsle linux_mips64 linux_mips64le \
  linux_loong64 linux_riscv64 linux_s390x \
  \
  openbsd_mips64

BOOTSTRAP_SOURCE:=go1.4-bootstrap-20171003.tar.gz
BOOTSTRAP_SOURCE_URL:=$(GO_SOURCE_URLS)
BOOTSTRAP_HASH:=f4ff5b5eb3a3cae1c993723f3eab519c5bae18866b5e5f96fe1102f0cb5c3e52

BOOTSTRAP_BUILD_DIR:=$(HOST_BUILD_DIR)/.go_bootstrap

BOOTSTRAP_GO_VALID_OS_ARCH:= \
  darwin_386     darwin_amd64 \
  dragonfly_386  dragonfly_amd64 \
  freebsd_386    freebsd_amd64    freebsd_arm \
  linux_386      linux_amd64      linux_arm \
  netbsd_386     netbsd_amd64     netbsd_arm \
  openbsd_386    openbsd_amd64 \
  plan9_386      plan9_amd64 \
                 solaris_amd64 \
  windows_386    windows_amd64

BOOTSTRAP_1_17_SOURCE:=go1.17.13.src.tar.gz
BOOTSTRAP_1_17_SOURCE_URL:=$(GO_SOURCE_URLS)
BOOTSTRAP_1_17_HASH:=a1a48b23afb206f95e7bbaa9b898d965f90826f6f1d1fc0c1d784ada0cd300fd

BOOTSTRAP_1_17_BUILD_DIR:=$(HOST_BUILD_DIR)/.go_bootstrap_1.17

BOOTSTRAP_1_20_SOURCE:=go1.20.14.src.tar.gz
BOOTSTRAP_1_20_SOURCE_URL:=$(GO_SOURCE_URLS)
BOOTSTRAP_1_20_HASH:=1aef321a0e3e38b7e91d2d7eb64040666cabdcc77d383de3c9522d0d69b67f4e

BOOTSTRAP_1_20_BUILD_DIR:=$(HOST_BUILD_DIR)/.go_bootstrap_1.20

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
include ../golang-compiler.mk
include ../golang-package.mk

PKG_UNPACK:=$(HOST_TAR) -C "$(PKG_BUILD_DIR)" --strip-components=1 -xzf "$(DL_DIR)/$(PKG_SOURCE)"
HOST_UNPACK:=$(HOST_TAR) -C "$(HOST_BUILD_DIR)" --strip-components=1 -xzf "$(DL_DIR)/$(PKG_SOURCE)"
BOOTSTRAP_UNPACK:=$(HOST_TAR) -C "$(BOOTSTRAP_BUILD_DIR)" --strip-components=1 -xzf "$(DL_DIR)/$(BOOTSTRAP_SOURCE)"
BOOTSTRAP_1_17_UNPACK:=$(HOST_TAR) -C "$(BOOTSTRAP_1_17_BUILD_DIR)" --strip-components=1 -xzf "$(DL_DIR)/$(BOOTSTRAP_1_17_SOURCE)"
BOOTSTRAP_1_20_UNPACK:=$(HOST_TAR) -C "$(BOOTSTRAP_1_20_BUILD_DIR)" --strip-components=1 -xzf "$(DL_DIR)/$(BOOTSTRAP_1_20_SOURCE)"

# don't strip ELF executables in test data
RSTRIP:=:
STRIP:=:

ifeq ($(GO_TARGET_SPECTRE_SUPPORTED),1)
  PKG_CONFIG_DEPENDS+=CONFIG_GOLANG_SPECTRE
endif

define Package/golang/Default
$(call GoPackage/GoSubMenu)
  TITLE:=Go programming language
  URL:=https://go.dev/
  DEPENDS:=$(GO_ARCH_DEPENDS)
endef

define Package/golang/Default/description
The Go programming language is an open source project to make
programmers more productive.

Go is expressive, concise, clean, and efficient. Its concurrency
mechanisms make it easy to write programs that get the most out of
multicore and networked machines, while its novel type system enables
flexible and modular program construction. Go compiles quickly to
machine code yet has the convenience of garbage collection and the power
of run-time reflection. It's a fast, statically typed, compiled language
that feels like a dynamically typed, interpreted language.
endef

# go tool requires source present:
# https://github.com/golang/go/issues/4635
define Package/golang
$(call Package/golang/Default)
  TITLE+= (compiler)
  DEPENDS+= +golang-src
endef

define Package/golang/description
$(call Package/golang/Default/description)

This package provides an assembler, compiler, linker, and compiled
libraries for the Go programming language.
endef

define Package/golang/config
  source "$(SOURCE)/Config.in"
endef

define Package/golang-doc
$(call Package/golang/Default)
  TITLE+= (documentation)
endef

define Package/golang-doc/description
$(call Package/golang/Default/description)

This package provides the documentation for the Go programming language.
endef

define Package/golang-src
$(call Package/golang/Default)
  TITLE+= (source files)
endef

define Package/golang-src/description
$(call Package/golang/Default/description)

This package provides the Go programming language source files needed
for cross-compilation.
endef


# Bootstrap

BOOTSTRAP_ROOT_DIR:=$(call qstrip,$(CONFIG_GOLANG_EXTERNAL_BOOTSTRAP_ROOT))

ifeq ($(BOOTSTRAP_ROOT_DIR),)
  BOOTSTRAP_ROOT_DIR:=$(BOOTSTRAP_BUILD_DIR)

  define Download/golang-bootstrap
    FILE:=$(BOOTSTRAP_SOURCE)
    URL:=$(BOOTSTRAP_SOURCE_URL)
    HASH:=$(BOOTSTRAP_HASH)
  endef
  $(eval $(call Download,golang-bootstrap))

  define Bootstrap/Prepare
	mkdir -p "$(BOOTSTRAP_BUILD_DIR)" && $(BOOTSTRAP_UNPACK) ;
  endef
  Hooks/HostPrepare/Post+=Bootstrap/Prepare

  $(eval $(call GoCompiler/AddProfile,Bootstrap,$(BOOTSTRAP_BUILD_DIR),,bootstrap,$(GO_HOST_OS_ARCH)))
endif


# Bootstrap 1.17

define Download/golang-bootstrap-1.17
  FILE:=$(BOOTSTRAP_1_17_SOURCE)
  URL:=$(BOOTSTRAP_1_17_SOURCE_URL)
  HASH:=$(BOOTSTRAP_1_17_HASH)
endef
$(eval $(call Download,golang-bootstrap-1.17))

define Bootstrap-1.17/Prepare
	mkdir -p "$(BOOTSTRAP_1_17_BUILD_DIR)" && $(BOOTSTRAP_1_17_UNPACK) ;
endef
Hooks/HostPrepare/Post+=Bootstrap-1.17/Prepare

$(eval $(call GoCompiler/AddProfile,Bootstrap-1.17,$(BOOTSTRAP_1_17_BUILD_DIR),,bootstrap-1.17,$(GO_HOST_OS_ARCH)))


# Bootstrap 1.20

define Download/golang-bootstrap-1.20
  FILE:=$(BOOTSTRAP_1_20_SOURCE)
  URL:=$(BOOTSTRAP_1_20_SOURCE_URL)
  HASH:=$(BOOTSTRAP_1_20_HASH)
endef
$(eval $(call Download,golang-bootstrap-1.20))

define Bootstrap-1.20/Prepare
	mkdir -p "$(BOOTSTRAP_1_20_BUILD_DIR)" && $(BOOTSTRAP_1_20_UNPACK) ;
endef
Hooks/HostPrepare/Post+=Bootstrap-1.20/Prepare

$(eval $(call GoCompiler/AddProfile,Bootstrap-1.20,$(BOOTSTRAP_1_20_BUILD_DIR),,bootstrap-1.20,$(GO_HOST_OS_ARCH)))

# Host

ifeq ($(GO_HOST_PIE_SUPPORTED),1)
  HOST_GO_ENABLE_PIE:=1
endif

# when using GO_LDFLAGS to set buildmode=pie, the PIE install suffix
# does not apply (we also delete the std lib during Host/Install)

$(eval $(call GoCompiler/AddProfile,Host,$(HOST_BUILD_DIR),$(HOST_GO_PREFIX),$(HOST_GO_VERSION_ID),$(GO_HOST_OS_ARCH),$(HOST_GO_INSTALL_SUFFIX)))

HOST_GO_VARS= \
	GOHOSTARCH="$(GO_HOST_ARCH)" \
	GOCACHE="$(GO_BUILD_CACHE_DIR)" \
	GOENV=off \
	CC="$(HOSTCC_NOCACHE)" \
	CXX="$(HOSTCXX_NOCACHE)"

define Host/Configure
	$(call GoCompiler/Bootstrap/CheckHost,$(BOOTSTRAP_GO_VALID_OS_ARCH))
	$(call GoCompiler/Host/CheckHost,$(HOST_GO_VALID_OS_ARCH))

	mkdir -p "$(GO_BUILD_CACHE_DIR)"
endef

define Host/Compile
	$(call GoCompiler/Bootstrap/Make, \
		$(HOST_GO_VARS) \
	)

	$(call GoCompiler/Bootstrap-1.17/Make, \
		GOROOT_BOOTSTRAP="$(BOOTSTRAP_ROOT_DIR)" \
		$(HOST_GO_VARS) \
	)

	$(call GoCompiler/Bootstrap-1.20/Make, \
		GOROOT_BOOTSTRAP="$(BOOTSTRAP_1_17_BUILD_DIR)" \
		$(HOST_GO_VARS) \
	)

	$(call GoCompiler/Host/Make, \
		GOROOT_BOOTSTRAP="$(BOOTSTRAP_1_20_BUILD_DIR)" \
		$(if $(HOST_GO_ENABLE_PIE),GO_LDFLAGS="-buildmode pie") \
		$(HOST_GO_VARS) \
	)
endef

# if host and target os/arch are the same,
# when go compiles a program, it will use the host std lib
# so remove it now and force go to rebuild std for target later
define Host/Install
	$(call Host/Uninstall)

	$(call GoCompiler/Host/Install/Bin,)
	$(call GoCompiler/Host/Install/Src,)

	$(call GoCompiler/Host/Install/BinLinks,)

	rm -rf "$(HOST_GO_ROOT)/pkg/$(GO_HOST_OS_ARCH)$(if $(HOST_GO_INSTALL_SUFFIX),_$(HOST_GO_INSTALL_SUFFIX))"

	$(INSTALL_DIR) "$(HOST_GO_ROOT)/openwrt"
	$(INSTALL_BIN) ./files/go-gcc-helper "$(HOST_GO_ROOT)/openwrt/"
	$(LN) go-gcc-helper "$(HOST_GO_ROOT)/openwrt/gcc"
	$(LN) go-gcc-helper "$(HOST_GO_ROOT)/openwrt/g++"
endef

define Host/Uninstall
	rm -rf "$(HOST_GO_ROOT)/openwrt"

	$(call GoCompiler/Host/Uninstall/BinLinks,)

	$(call GoCompiler/Host/Uninstall,)
endef


# Target

ifeq ($(GO_PKG_ENABLE_PIE),1)
  PKG_GO_INSTALL_SUFFIX:=$(GO_TARGET_PIE_INSTALL_SUFFIX)
endif

$(eval $(call GoCompiler/AddProfile,Package,$(PKG_BUILD_DIR),$(PKG_GO_PREFIX),$(PKG_GO_VERSION_ID),$(GO_OS_ARCH),$(PKG_GO_INSTALL_SUFFIX)))

PKG_GO_ZBOOTSTRAP_MODS:= \
	s/defaultGO386 = `[^`]*`/defaultGO386 = `$(or $(GO_386),sse2)`/; \
	s/defaultGOAMD64 = `[^`]*`/defaultGOAMD64 = `$(or $(GO_AMD64),v1)`/; \
	s/defaultGOARM = `[^`]*`/defaultGOARM = `$(or $(GO_ARM),7)`/; \
	s/defaultGOMIPS = `[^`]*`/defaultGOMIPS = `$(or $(GO_MIPS),hardfloat)`/; \
	s/defaultGOMIPS64 = `[^`]*`/defaultGOMIPS64 = `$(or $(GO_MIPS64),hardfloat)`/; \
	s/defaultGOPPC64 = `[^`]*`/defaultGOPPC64 = `$(or $(GO_PPC64),power8)`/;

PKG_GO_ZBOOTSTRAP_PATH:=$(PKG_BUILD_DIR)/src/internal/buildcfg/zbootstrap.go

PKG_GO_VARS= \
	GOHOSTARCH="$(GO_HOST_ARCH)" \
	GOCACHE="$(GO_BUILD_CACHE_DIR)" \
	GOENV=off \
	GO_GCC_HELPER_PATH="$$$$PATH" \
	CC=gcc \
	CXX=g++ \
	PKG_CONFIG=pkg-config \
	PATH="$(HOST_GO_ROOT)/openwrt:$$$$PATH"

PKG_GO_GCFLAGS= \
	$(if $(GO_PKG_ENABLE_SPECTRE),-spectre all)

PKG_GO_ASMFLAGS= \
	$(if $(GO_PKG_ENABLE_SPECTRE),-spectre all)

PKG_GO_LDFLAGS= \
	-buildid '$(SOURCE_DATE_EPOCH)' \
	-linkmode external \
	-extldflags '$(patsubst -z%,-Wl$(comma)-z$(comma)%,$(TARGET_LDFLAGS))' \
	$(if $(CONFIG_NO_STRIP)$(CONFIG_DEBUG),,-s -w)

PKG_GO_INSTALL_ARGS= \
	-buildvcs=false \
	-trimpath \
	-ldflags "all=$(PKG_GO_LDFLAGS)" \
	$(if $(PKG_GO_GCFLAGS),-gcflags "all=$(PKG_GO_GCFLAGS)") \
	$(if $(PKG_GO_ASMFLAGS),-asmflags "all=$(PKG_GO_ASMFLAGS)") \
	$(if $(filter $(GO_PKG_ENABLE_PIE),1),-buildmode pie)

define Build/Configure
	mkdir -p "$(GO_BUILD_CACHE_DIR)"
endef

define Build/Compile
	@echo "Building target Go first stage"

	$(call GoCompiler/Package/Make, \
		GOROOT_BOOTSTRAP="$(HOST_GO_ROOT)" \
		GO_GCC_HELPER_CC="$(HOSTCC)" \
		GO_GCC_HELPER_CXX="$(HOSTCXX)" \
		$(PKG_GO_VARS) \
	)

	$(SED) '$(PKG_GO_ZBOOTSTRAP_MODS)' "$(PKG_GO_ZBOOTSTRAP_PATH)"

	( \
		if echo 'int main() { return 0; }' | $(TARGET_CC) -o $(PKG_BUILD_DIR)/test-ldso -x c - > /dev/null 2>&1; then \
			LDSO=$$$$( \
				readelf -l $(PKG_BUILD_DIR)/test-ldso | \
				sed -n -e 's/^.*interpreter: \(.*\)[]]/\1/p' \
			) ; \
		fi ; \
		$(SED) "s,defaultGO_LDSO = \`[^\`]*\`,defaultGO_LDSO = \`$$$$LDSO\`," "$(PKG_GO_ZBOOTSTRAP_PATH)" ; \
	)

	@echo "Building target Go second stage"

	( \
		cd "$(PKG_BUILD_DIR)/bin" ; \
		export $(GO_PKG_TARGET_VARS) ; \
		$(CP) go go-host ; \
		GO_GCC_HELPER_CC="$(TARGET_CC)" \
		GO_GCC_HELPER_CXX="$(TARGET_CXX)" \
		$(PKG_GO_VARS) \
		./go-host install -a $(PKG_GO_INSTALL_ARGS) std cmd ; \
		retval="$$$$?" ; \
		rm -f go-host ; \
		exit "$$$$retval" ; \
	)
endef

define Package/golang/install
	$(call GoCompiler/Package/Install/Bin,$(1)$(PKG_GO_PREFIX))
	$(call GoCompiler/Package/Install/BinLinks,$(1)$(PKG_GO_PREFIX))
endef

define Package/golang-doc/install
	$(call GoCompiler/Package/Install/Doc,$(1)$(PKG_GO_PREFIX))
endef

define Package/golang-src/install
	$(call GoCompiler/Package/Install/Src,$(1)$(PKG_GO_PREFIX))
endef

# src/debug contains ELF executables as test data
# and they reference these libraries
# we need to call this in Package/$(1)/extra_provides
# to pass CheckDependencies in include/package-ipkg.mk
define Package/golang-src/extra_provides
	echo 'libc.so.6'
endef


$(eval $(call HostBuild))
$(eval $(call BuildPackage,golang))
$(eval $(call BuildPackage,golang-doc))
$(eval $(call BuildPackage,golang-src))
