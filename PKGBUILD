# Maintainer: Boohbah <boohbah@gmail.com>
# Contributor: Cian Mc Govern <cian@cianmcgovern.com>
# Contributor: Mikael Eriksson <mikael_eriksson@miffe.org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

pkgname=linux-git-nvidia
pkgver=313.18
_extramodules=extramodules-3.8-git
pkgrel=1
pkgdesc="NVIDIA drivers for linux-git"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux-git>=20130126' "nvidia-utils=${pkgver}")
makedepends=('linux-git-headers>=20130126')
conflicts=('nvidia-96xx' 'nvidia-173xx')
license=('custom')
install=nvidia.install
options=(!strip)

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('780c37c28a6e06e9571cafe348b7da64')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('fa17a260793a38b4b8ae367db2e03b39')
fi

source=(${source} "conftest.patch")
md5sums=(${md5sums} "0dff3c0d84aadeb2e3af6ede088800f7")

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}/kernel"
    patch < "${srcdir}/conftest.patch"
    make SYSSRC=/usr/src/"linux-${_kernver}/" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
