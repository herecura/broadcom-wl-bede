# Maintainer: graysky <graysky AT archlinux DOT org>
# Maintainer: Armin K. <krejzi at email dot com>
# Contributor: Austin ( doorknob60 [at] gmail [dot] com )
# Contributor: Gaetan Bisson <bisson@archlinux.org>

pkgname=broadcom-wl-bede
pkgver=6.30.223.248
pkgrel=1
_pkgdesc='Broadcom 802.11abgn hybrid Linux networking device driver for linux-bede'
_extramodules=3.18-BEDE-external
pkgdesc="${_pkgdesc}"
arch=('i686' 'x86_64')
url='http://www.broadcom.com/support/802.11/linux_sta.php'
license=('custom')
depends=('linux-bede>=3.18.5' 'linux-bede<3.19')
makedepends=('linux-bede-headers>=3.18' 'linux-bede-headers<3.19')
source=('modprobe.d'
'license.patch'
'linux-recent.patch'
'unfuck.patch'
'broadcom-sta-6.30.223.248-linux-3.18-null-pointer-crash.patch'
'gcc.patch')
source_i686+=("http://www.broadcom.com/docs/linux_sta/hybrid-v35-nodebug-pcoem-${pkgver//./_}.tar.gz")
source_x86_64+=("http://www.broadcom.com/docs/linux_sta/hybrid-v35_64-nodebug-pcoem-${pkgver//./_}.tar.gz")
sha256sums=('b4aca51ac5ed20cb79057437be7baf3650563b7a9d5efc515f0b9b34fbb9dc32'
            '2f70be509aac743bec2cc3a19377be311a60a1c0e4a70ddd63ea89fae5df08ac'
            'f651681496316ac60b5f2d37c93a36b3a4a1ee29ab6aada6eebaef7f7c1f1d02'
            'f9ba1c302fcb2b918ca0386917cbab9f3f8fe80c854fbe378edd4acad46d7b74'
            '6931a6f870b482a702a714ddab41cbfcbb606bf9bbef6d046605b905fc96b8f9'
            'b07ce80f2e079cce08c8ec006dda091f6f73f158c8a62df5bac2fbabb6989849')
sha256sums_i686=('b196543a429c22b2b8d75d0c1d9e6e7ff212c3d3e1f42cc6fd9e4858f01da1ad')
sha256sums_x86_64=('3d994cc6c05198f4b6f07a213ac1e9e45a45159899e6c4a7feca5e6c395c3022')

install=broadcom-wl-bede.install

prepare() {
	patch -p1 -i linux-recent.patch
	patch -p1 -i license.patch
	patch -p1 -i gcc.patch
	patch -p1 -i unfuck.patch
	patch -p1 -i broadcom-sta-6.30.223.248-linux-3.18-null-pointer-crash.patch

	sed -e "/BRCM_WLAN_IFNAME/s:eth:wlan:" -i src/wl/sys/wl_linux.c
}


build() {
	_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
	make -C /usr/lib/modules/"${_kernver}"/build M=$(pwd)
}

package() {
	install -Dm644 wl.ko "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"

	# makepkg does not do this automatically for this pkg so do it here
	gzip -9 "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"
	install -Dm644 lib/LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 modprobe.d "${pkgdir}/usr/lib/modprobe.d/broadcom-wl_ck.conf"
}
