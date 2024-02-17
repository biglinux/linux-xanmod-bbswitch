# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

_linuxprefix=linux-xanmod
_kernver="$(cat /usr/src/${_linuxprefix}/version)"
pkgname=$_linuxprefix-bbswitch
_pkgname=bbswitch
pkgver=0.8
pkgrel=67510
pkgdesc="kernel module allowing to switch dedicated graphics card on Optimus laptops"
arch=('x86_64')
url="http://github.com/Bumblebee-Project/bbswitch"
license=('GPL')
depends=("$_linuxprefix")
makedepends=("$_linuxprefix-headers")
provides=("$_pkgname=$pkgver")
groups=("$_linuxprefix-extramodules")
source=("$pkgname-$pkgver.tar.gz::https://github.com/Bumblebee-Project/bbswitch/archive/v${pkgver}.tar.gz"
        'kernel57.patch' 'kernel518.patch')
sha256sums=('76cabd3f734fb4fe6ebfe3ec9814138d0d6f47d47238521ecbd6a986b60d1477'
            '3b8039f3cd32d2aa8ad0b2426f28faac218eacd134c1e39454c9feca9d612789'
            '04061ecbee433de137d8e68cd42271a30c172bb87829cf350d50df1b24414139')

prepare() {
  cd ${_pkgname}-${pkgver}

  local src
  for src in "${source[@]}"; do
      src="${src%%::*}"
      src="${src##*/}"
      [[ $src = *.patch ]] || continue
      msg2 "Applying patch: $src..."
      patch -Np1 < "../$src"
  done
}

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  cd ${_pkgname}-${pkgver}
  # KDIR is necessary even when cleaning
  make KDIR=/usr/lib/modules/${_kernver}/build
}

package() {
  cd ${_pkgname}-${pkgver}
  install -D -m 644 bbswitch.ko $pkgdir/usr/lib/modules/${_kernver}/extramodules/bbswitch.ko
  # gzip -9 modules
  find "$pkgdir" -name '*.ko' -exec gzip -9 {} \;
}
