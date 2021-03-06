# Maintainer: Jose Riha <jose1711 gmail com>
# Maintainer: Sebastian J. Bronner <waschtl@sbronner.com>
# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Contributor: Christoph Vigano <mail@cvigano.de>
# Contributor: Etienne Wodey <airwoodix@posteo.me>

pkgname=st-airwoodix
_pkgname=st
provides=("${_pkgname}")
conflicts=("${_pkgname}")
pkgver=0.8.2
pkgrel=10
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64' 'armv7h')
license=('MIT')
depends=(libxft ttf-fira-code)
url=https://st.suckless.org
_theme_hdr=base16-atelier-forest-theme.h
source=(https://dl.suckless.org/$_pkgname/$_pkgname-$pkgver.tar.gz
        terminfo.patch
	style.patch
        README.terminfo.rst
	https://raw.githubusercontent.com/honza/base16-st/b3d0d4fbdf86d9b3eda06f42a5bdf261b1f7d1d1/build/"$_theme_hdr")
sha256sums=('aeb74e10aa11ed364e1bcc635a81a523119093e63befd2f231f8b0705b15bf35'
            'bf6c8b73a606a8e513c7919d91f93ed7aeb5f44e80269bb244cc01659145a5ea'
            '8e5f2f0c711dad9e28a0a795539a1e10e7a82814f3c520842aa05f5ddded6ad0'
            '0ebcbba881832adf9c98ce9fe7667c851d3cc3345077cb8ebe32702698665be2'
            '137c63386a7e37a3cc6817a4c4013aff844216b562e39919fd025265c6b75920')
_sourcedir=$_pkgname-$pkgver
_makeopts="--directory=$_sourcedir"

prepare() {
    patch --directory="$_sourcedir" --strip=0 < terminfo.patch

    cp "$_sourcedir/config.def.h" "$BUILDDIR"

    patch "$BUILDDIR/config.def.h" --strip=0 < style.patch
    sed -i "s/_THEME_HEADER_/$_theme_hdr/" "$BUILDDIR/config.def.h"
    cp "$_theme_hdr" "$_sourcedir/"

    cp "$BUILDDIR/config.def.h" "$_sourcedir/config.h"
}

build() {
  make $_makeopts X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  local installopts='--mode 0644 -D --target-directory'
  local shrdir="$pkgdir/usr/share"
  local licdir="$shrdir/licenses/$pkgname"
  local docdir="$shrdir/doc/$pkgname"
  make $_makeopts PREFIX=/usr DESTDIR="$pkgdir" install
  install $installopts "$licdir" "$_sourcedir/LICENSE"
  install $installopts "$docdir" "$_sourcedir/README"
  install $installopts "$docdir" README.terminfo.rst
  install $installopts "$shrdir/$pkgname" "$_sourcedir/st.info"
}
