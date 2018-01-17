# Maintainer: graysky <graysky AT archlinux DOT org>
# Maintainer: Armin K. <krejzi at email dot com>
# Contributor: Austin ( doorknob60 [at] gmail [dot] com )
# Contributor: Gaetan Bisson <bisson@archlinux.org>

pkgname=broadcom-wl-bede
pkgver=6.30.223.271
pkgrel=156
_pkgdesc='Broadcom 802.11abgn hybrid Linux networking device driver for linux-bede'
_extramodules=4.14-BEDE-external
_current_linux_version=4.14.14
_next_linux_version=4.15
pkgdesc="${_pkgdesc}"
arch=('x86_64')
url='http://www.broadcom.com/support/802.11/linux_sta.php'
license=('custom')
makedepends=(
    "linux-bede>=$_current_linux_version"
    "linux-bede-headers>=$_current_linux_version"
    "linux-bede<$_next_linux_version"
    "linux-bede-headers<$_next_linux_version"
)
source=(
    'modprobe.d'
    '001-null-pointer-fix.patch'
    '002-rdtscl.patch'
    '003-linux47.patch'
    '004-linux48.patch'
    '005-debian-fix-kernel-warnings.patch'
    '006-linux411.patch'
    '007-linux412.patch'
)
source_i686=("http://www.broadcom.com/docs/linux_sta/hybrid-v35-nodebug-pcoem-${pkgver//./_}.tar.gz")
source_x86_64=("http://www.broadcom.com/docs/linux_sta/hybrid-v35_64-nodebug-pcoem-${pkgver//./_}.tar.gz")
sha256sums=('b4aca51ac5ed20cb79057437be7baf3650563b7a9d5efc515f0b9b34fbb9dc32'
            '32e505a651fdb9fd5e4870a9d6de21dd703dead768c2b3340a2ca46671a5852f'
            '4ea03f102248beb8963ad00bd3e36e67519a90fa39244db065e74038c98360dd'
            '30ce1d5e8bf78aee487d0f3ac76756e1060777f70ed1a9cf95215c3a52cfbe2e'
            '833af3b209d6a101d9094db16480bda2ad9a85797059b0ae0b13235ad3818e9c'
            '2306a59f9e7413f35a0669346dcd05ef86fa37c23b566dceb0c6dbee67e4d299'
            '977b1663ce055860b0b60e7cf882658f507d81909f935d1a8b785896f64176e8'
            'a3d13e8abb96ad440dbfae29acae82d31d1ced2ea62052f1efb2c3c4add347ce')
sha256sums_x86_64=('5f79774d5beec8f7636b59c0fb07a03108eef1e3fd3245638b20858c714144be')

prepare() {
    patch -p1 -i "$srcdir/001-null-pointer-fix.patch"
    patch -p1 -i "$srcdir/002-rdtscl.patch"
    patch -p1 -i "$srcdir/003-linux47.patch"
    patch -p1 -i "$srcdir/004-linux48.patch"
    patch -p1 -i "$srcdir/005-debian-fix-kernel-warnings.patch"
    patch -p1 -i "$srcdir/006-linux411.patch"
    patch -p1 -i "$srcdir/007-linux412.patch"

	sed -e "/BRCM_WLAN_IFNAME/s:eth:wlan:" -i src/wl/sys/wl_linux.c
}


build() {
	_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
	make -C /usr/lib/modules/"${_kernver}"/build M=$(pwd)
}

package() {
	depends=(
        "linux-bede>=$_current_linux_version"
        "linux-bede<$_next_linux_version"
    )

	install -Dm644 wl.ko "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"

	# makepkg does not do this automatically for this pkg so do it here
	gzip -9 "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"
	install -Dm644 lib/LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 modprobe.d "${pkgdir}/usr/lib/modprobe.d/broadcom-wl-bede.conf"
}
