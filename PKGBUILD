# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux71

pkgname="${_linuxprefix}-nvidia-470xx"
pkgver=470.256.02
pkgrel=0.1
pkgdesc="NVIDIA kernel modules for ${_linuxprefix}"
arch=('x86_64')
url="https://www.nvidia.com/"
license=('custom')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}" "nvidia-utils=${pkgver}" 'libglvnd')
makedepends=("${_linuxprefix}-headers" "nvidia-dkms=$pkgver")
provides=("nvidia=${pkgver}" 'NVIDIA-MODULE')
options=(!strip)
source=('linux70.patch')
sha256sums=('7cc9af3a95443a88df3f998c94a9ce26e9c113a6e451b60b2d92528279d8c943')

prepare() {
  mkdir -p nvidia/${pkgver}/source_patched
  cp -av /usr/src/nvidia-${pkgver}/* nvidia/${pkgver}/source_patched
  cd nvidia/${pkgver}/source_patched
  patch -p1 -i $srcdir/linux70.patch
  cd ..
  ln -sfv source_patched source
}

build() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    fakeroot dkms build --dkmstree "${srcdir}" -m nvidia/${pkgver} -k ${_kernver}
}

package() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    install -Dt "${pkgdir}/usr/lib/modules/${_kernver}/extramodules" -m644 nvidia/${pkgver}/${_kernver}/${CARCH}/module/*.ko.zst

    # compress each module individually
    find "$pkgdir" -name '*.ko' -exec zstd --rm -19 {} +

    install -Dm644 /usr/share/licenses/nvidia-470xx-dkms/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
