# vim:set ft=sh et:
# Maintainer: graysky <graysky AT archlinux DOT org>
# Maintainer: Armin K. <krejzi at email dot com>
# Contributor: Austin ( doorknob60 [at] gmail [dot] com )
# Contributor: Gaetan Bisson <bisson@archlinux.org>

_pkgname=broadcom-wl
pkgname=$_pkgname-bede
pkgver=6.30.223.271
pkgrel=432
_pkgdesc='Broadcom 802.11abgn hybrid Linux networking device driver for linux-bede'
_current_linux_version=5.8.2
_next_linux_version=5.9
pkgdesc="${_pkgdesc}"
arch=('x86_64')
url='http://www.broadcom.com/support/802.11/linux_sta.php'
license=('custom')
makedepends=(
    "linux-bede>=$_current_linux_version"
    "linux-bede-headers>=$_current_linux_version"
    "linux-bede<$_next_linux_version"
    "linux-bede-headers<$_next_linux_version"
    "broadcom-wl-dkms>=$pkgver"
)
source=('modprobe.d')
sha512sums=('c8d6bd80935526bac911938b097575dee3c631f351e8f81df537abb613d98cadc68c449dadf39e23e656bfb4676d97503c9f630bd6b283feb77c998e26e50a84')

package() {
	depends=(
        "linux-bede>=$_current_linux_version"
        "linux-bede<$_next_linux_version"
    )

    local kernver=$(</usr/src/linux-bede/version)
    local extradir="/usr/lib/modules/$kernver/extramodules"
    install -dm755 "${pkgdir}${extradir}/$_pkgname"
    cp -a "/var/lib/dkms/$_pkgname/kernel-$kernver-x86_64/module"/* \
        "${pkgdir}${extradir}/$_pkgname/"

	install -Dm644 "${srcdir}/modprobe.d" \
        "${pkgdir}/usr/lib/modprobe.d/broadcom-wl-bede.conf"
}
