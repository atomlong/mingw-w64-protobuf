pkgname=('mingw-w64-protobuf')
_pkgname=protobuf
pkgver=3.11.2
pkgrel=1
pkgdesc="Protocol Buffers - Google's data interchange format (mingw-w64)"
arch=('any')
url='https://developers.google.com/protocol-buffers/'
license=('BSD')
depends=('mingw-w64-crt' 'mingw-w64-zlib')
makedepends=('mingw-w64-configure' 'protobuf' 'unzip')
options=(!strip !buildflags staticlibs)
source=("https://github.com/google/protobuf/archive/v${pkgver}.tar.gz")
sha256sums=('e8c7601439dbd4489fe5069c33d374804990a56c2f710e00227ee5d8fd650e67')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  cd ${srcdir}/${_pkgname}-${pkgver}
  ./autogen.sh
  for _arch in ${_architectures}; do
    mkdir -p build-${_arch} && pushd build-${_arch}
    LIBS="-lssp" ${_arch}-configure --with-protoc=/usr/bin/protoc ..
    make
    popd
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}"/${_pkgname}-${pkgver}/build-${_arch}
    make DESTDIR="$pkgdir" install
    rm "${pkgdir}"/usr/${_arch}/bin/*.exe
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
  done
}
