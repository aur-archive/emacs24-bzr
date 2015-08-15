# Contributor: Selman Ulug <selman.ulug@gmail.com>
# Maintainer: Stefan Husmann <stefan-husmann@t-online.de>

pkgname=emacs24-bzr
pkgver=117624
pkgrel=1
pkgdesc='The extensible, customizable, self-documenting real-time display editor (emacs-24 branch)'
arch=('i686' 'x86_64')
url='http://www.gnu.org/software/emacs/emacs.html'
license=('GPL3')
depends=('librsvg' 'gpm' 'giflib' 'libxpm' 'gtk2' 'hicolor-icon-theme' 'gconf' 'desktop-file-utils' 'imagemagick')
makedepends=('bzr')
provides=('emacs=24.4.51.1')
conflicts=('emacs')
source=("emacs-24::bzr+http://bzr.savannah.gnu.org/r/emacs/emacs-24/")
install=emacs.install
md5sums=('SKIP')
_bzrmod=emacs-24

pkgver() {
  cd $srcdir/${_bzrmod}
  bzr revno
}

build() {
  cd "$srcdir"/${_bzrmod}
  ./autogen.sh
  ./configure --disable-maintainer-mode \
      --prefix=/usr \
      --sysconfdir=/etc \
      --libexecdir=/usr/lib \
      --localstatedir=/var \
      --with-x-toolkit=gtk3 \
      --with-xft \
      --without-sound
  make bootstrap
}

package() {
  cd "$srcdir/${_bzrmod}"
  make DESTDIR="$pkgdir" install

  # remove conflict with ctags package
  mv "$pkgdir"/usr/bin/{ctags,ctags.emacs}
  mv "$pkgdir"/usr/share/man/man1/{ctags.1,ctags.emacs.1}.gz
  # remove conflict with the texinfo-package
  rm $pkgdir/usr/share/info/info.info.gz
  # fix all the 777 perms on directories
  find "$pkgdir"/usr/share/emacs/$_majorver -type d -exec chmod 755 {} \;
  # fix user/root permissions on usr/share files
  find "$pkgdir"/usr/share/emacs/$_majorver -exec chown root:root {} \;
  # fix perms on /var/games
  chmod 775 "$pkgdir"/var/games
  chmod 775 "$pkgdir"/var/games/emacs
  chmod 664 "$pkgdir"/var/games/emacs/*
  chown -R root:games "$pkgdir"/var/games
}
