pkgname=dmenu
pkgver=4.7
pkgrel=1
pkgdesc="A generic menu for X with various patches applied"
url="https://tools.suckless.org/dmenu/"
arch=('i686' 'x86_64')
license=('MIT')
depends=('sh' 'libxinerama' 'libxft')
makedepends=('git')
provides=($pkgname)
conflicts=($pkgname)
groups=('modified')

source=(https://dl.suckless.org/tools/dmenu-$pkgver.tar.gz
		https://tools.suckless.org/dmenu/patches/xyw/dmenu-xyw-$pkgver.diff
		https://tools.suckless.org/dmenu/patches/password/dmenu-password-$pkgver.diff
		https://tools.suckless.org/dmenu/patches/line-height/dmenu-lineheight-$pkgver.diff
		https://tools.suckless.org/dmenu/patches/mouse-support/dmenu-mousesupport-$pkgver.diff
		config.h)

md5sums=('57b5c28f7e3de908f5e5c2a65cd72e16'
         '51398d7c7fd1782bed57516f4b22de70'
         'b7edb3ef4143f4caafb632897452222e'
         '0e4017e122a52a4ccbea5db54ab0da6a'
         '274972c3f6de489dd00543a6d653b960'
         '2c3ce170ede8322a2004fe3f3ed1e053')

prepare() {
	# use custom config if exists
	cd $srcdir
	if [[ -f config.h ]]; then
		cp "config.h" "$pkgname-$pkgver/config.def.h"
	fi

	# apply patches
	cd $srcdir/$pkgname-$pkgver
	patch -p1 -F 3 --input="${srcdir}/dmenu-lineheight-$pkgver.diff"
	patch -p1 --forward --input="${srcdir}/dmenu-xyw-$pkgver.diff"
	patch -p1 --input="${srcdir}/dmenu-mousesupport-$pkgver.diff"
	patch -p1 -F 5 --input="${srcdir}/dmenu-password-$pkgver.diff"

	# rename config.def.h to config.h
	mv config.def.h config.h
}

build() {
	cd $srcdir/$pkgname-$pkgver
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
	cd $srcdir/$pkgname-$pkgver
	make DESTDIR="$pkgdir" PREFIX=/usr install
	install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
