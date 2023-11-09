# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

_linuxprefix=linux-xanmod
_extramodules=$(find /usr/lib/modules -type d -iname 6.5.11*xanmod* | rev | cut -d "/" -f1 | rev)
pkgname=$_linuxprefix-bbswitch
_pkgname=bbswitch
pkgver=0.8
pkgrel=65111
pkgdesc="kernel module allowing to switch dedicated graphics card on Optimus laptops"
arch=('x86_64')
url="http://github.com/Bumblebee-Project/bbswitch"
license=('GPL')
depends=("$_linuxprefix")
makedepends=("$_linuxprefix-headers")
provides=("$_pkgname=$pkgver")
groups=("$_linuxprefix-extramodules")
install=bbswitch.install
source=("$pkgname-$pkgver.tar.gz::https://github.com/Bumblebee-Project/bbswitch/archive/v${pkgver}.tar.gz"
        'kernel57.patch' 'kernel518.patch')
sha256sums=('76cabd3f734fb4fe6ebfe3ec9814138d0d6f47d47238521ecbd6a986b60d1477'
            '3b8039f3cd32d2aa8ad0b2426f28faac218eacd134c1e39454c9feca9d612789'
            '04061ecbee433de137d8e68cd42271a30c172bb87829cf350d50df1b24414139')

prepare() {
  cd ${_pkgname}-${pkgver}
  patch -p1 -i ../kernel57.patch
  patch -p1 -i ../kernel518.patch
}

build() {
  _kernver=$(find /usr/lib/modules -type d -iname 6.5.11*xanmod* | rev | cut -d "/" -f1 | rev)

  cd ${_pkgname}-${pkgver}
  # KDIR is necessary even when cleaning
  make KDIR=/usr/lib/modules/${_kernver}/build
}

package() {
  cd ${_pkgname}-${pkgver}
  install -D -m 644 bbswitch.ko $pkgdir/usr/lib/modules/${_extramodules}/bbswitch.ko
  # gzip -9 modules
  find "$pkgdir" -name '*.ko' -exec gzip -9 {} \;
  sed -i -e "s/EXTRAMODULES=.*/EXTRAMODULES=${_extramodules}/g" $startdir/*.install
}
