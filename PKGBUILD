# Contributor: Meowdib <meowdib at gmail dot com>
# Maintainer: Doug <doug.quaid@gmail.com>

pkgname=nvidia-lqx-3.1
pkgver=290.10
_kernver='3.1.0-lqx'
pkgrel=1
pkgdesc="NVIDIA drivers for linux-lqx."
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=("linux-lqx>=3.1" "linux-lqx<3.2" "nvidia-utils=${pkgver}")
conflicts=()
license=('custom')
install=nvidia-lqx.install
options=(!strip)

if [ "$CARCH" = "i686" ]; then
	_arch='x86'
	_pkg="NVIDIA-Linux-${_arch}-${pkgver}"
        source=("http://us.download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
	md5sums=('50319a4b3818c12c9c7243525e0e6316')
elif [ "$CARCH" = "x86_64" ]; then
	_arch='x86_64'
	_pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
        source=("http://us.download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
	md5sums=('cebfba9a7e91716a06c66bb5b38d9661')
fi

build() {
    cd $srcdir
    sh ${_pkg}.run --extract-only
    cd ${_pkg}/kernel
    make SYSSRC=/lib/modules/${_kernver}/build module || return 1
}

package() {
	install -D -m644 "$srcdir"/$_pkg/kernel/nvidia.ko \
		"$pkgdir"/lib/modules/$_kernver/kernel/drivers/video/nvidia.ko
	install -d -m755 "$pkgdir"/etc/modprobe.d
	echo "blacklist nouveau" >> "$pkgdir"/etc/modprobe.d/nouveau_blacklist_ck.conf
	sed -i -e "s/KERNEL_VERSION='.*'/KERNEL_VERSION='$_kernver'/" "$startdir/nvidia-lqx.install"
	gzip "${pkgdir}/lib/modules/${_kernver}/kernel/drivers/video/nvidia.ko"
}
