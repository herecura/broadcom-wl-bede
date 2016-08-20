# Maintainer: graysky <graysky AT archlinux DOT org>
# Maintainer: Armin K. <krejzi at email dot com>
# Contributor: Austin ( doorknob60 [at] gmail [dot] com )
# Contributor: Gaetan Bisson <bisson@archlinux.org>

pkgname=broadcom-wl-bede
pkgver=6.30.223.271
pkgrel=28
_pkgdesc='Broadcom 802.11abgn hybrid Linux networking device driver for linux-bede'
_extramodules=4.7-BEDE-external
_current_linux_version=4.7.2
_next_linux_version=4.8
pkgdesc="${_pkgdesc}"
arch=('i686' 'x86_64')
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
    'linux-4.3.patch'
    'linux47.patch'
)
source_i686=("http://www.broadcom.com/docs/linux_sta/hybrid-v35-nodebug-pcoem-${pkgver//./_}.tar.gz")
source_x86_64=("http://www.broadcom.com/docs/linux_sta/hybrid-v35_64-nodebug-pcoem-${pkgver//./_}.tar.gz")
sha256sums=('b4aca51ac5ed20cb79057437be7baf3650563b7a9d5efc515f0b9b34fbb9dc32'
            '32e505a651fdb9fd5e4870a9d6de21dd703dead768c2b3340a2ca46671a5852f'
            'ea44e75fc93fd73ec67db639fc77b7b9bb714fadb3ce29f8553e4adc3dc71834'
            '30ce1d5e8bf78aee487d0f3ac76756e1060777f70ed1a9cf95215c3a52cfbe2e')
sha256sums_i686=('4f8b70b293ac8cc5c70e571ad5d1878d0f29d133a46fe7869868d9c19b5058cd')
sha256sums_x86_64=('5f79774d5beec8f7636b59c0fb07a03108eef1e3fd3245638b20858c714144be')

install=broadcom-wl-bede.install

prepare() {
    patch -p1 -i "$srcdir/001-null-pointer-fix.patch"
    patch -p1 -i "$srcdir/linux-4.3.patch"
    patch -p1 -i "$srcdir/linux47.patch"

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

    sed -i -e "s/EXTRAMODULES=.*/EXTRAMODULES='$_extramodules'/" "$startdir/broadcom-wl-bede.install"
}
