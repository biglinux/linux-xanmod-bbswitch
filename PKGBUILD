# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Maintainer: Bernhard Landauer <bernhard@manjaro.org>

_linuxprefix=linux-xanmod

_module=bbswitch
pkgname="${_linuxprefix}-${_module}"
pkgver=0.8
pkgrel=61171
pkgdesc="Kernel module allowing to switch dedicated graphics card on Optimus laptops"
arch=('x86_64')
url="http://github.com/Bumblebee-Project/bbswitch"
license=('GPL')
depends=("${_linuxprefix}")
makedepends=("${_linuxprefix}-headers")
groups=("${_linuxprefix}-extramodules")
source=("$pkgname-$pkgver.tar.gz::https://github.com/Bumblebee-Project/bbswitch/archive/v${pkgver}.tar.gz"
        '0001-proc_ops-struct.patch'
        '0002-kernel-5.7.patch'
        '0003-kernel-5.18.patch')
sha256sums=('76cabd3f734fb4fe6ebfe3ec9814138d0d6f47d47238521ecbd6a986b60d1477'
            '41fd734ffebbb98a15732258c18ad20e621d8b002600d77b273c572c6e82a3e2'
            'e7bb0256c5d4304fdd10efa4ef1c626737b78093ac38cad26d89adcab05a32ab'
            '04061ecbee433de137d8e68cd42271a30c172bb87829cf350d50df1b24414139')

prepare() {
  cd "${_module}-${pkgver}"
  patch -Np1 < ../0001-proc_ops-struct.patch
  patch -Np1 < ../0002-kernel-5.7.patch
  patch -Np1 < ../0003-kernel-5.18.patch
}

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  cd "${_module}-${pkgver}"
  make KDIR="/usr/lib/modules/${_kernver}/build"
}

package() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  cd "${_module}-${pkgver}"
  install -Dm644 *.ko -t "$pkgdir/usr/lib/modules/${_kernver}/extramodules/"
  find "${pkgdir}" -name '*.ko' -exec strip --strip-debug {} +
  find "${pkgdir}" -name '*.ko' -exec xz {} +
}
