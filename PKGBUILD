# Maintainer: Jan Dolinar <dolik.rce@gmail.com>

pkgname=upp-svn
pkgver=7400
pkgrel=1
pkgdesc="Radical and innovative multiplatform C++ framework (known as U++)"
arch=('any')
url="http://www.ultimatepp.org"
license=('BSD')
groups=()
depends=('gcc-libs' 'libpng' 'libxft' 'theide')
makedepends=('subversion')
optdepends=('libnotify: Enables compiling gtk-styled apps')
provides=('upp')
conflicts=('upp')
replaces=()
backup=()
options=(emptydirs !strip)
install=
source=('GCC.bm' 'license.txt')
noextract=()

_svntrunk="http://upp-mirror.googlecode.com/svn/trunk/"

prepare() {
  # many users have already working copy of U++ on their system, so they
  # can use it for building (e.g. to save network traffic or to speed up
  # things) by setting $UPPSVN environment variable
  if [ "x$UPPSVN" != "x" ] && svn info "$UPPSVN" &> /dev/null
  then
    _svnmod=$UPPSVN
  else
    _svnmod="$srcdir/uppsvn"
  fi
}

build() {
  cd "$srcdir"
  #get sources
  msg "Downloading sources from svn..."
  for n in bazaar reference examples tutorial uppsrc
  do
    msg2 "$n"
    if [ -d "$_svnmod/$n" ]; then
      (cd $_svnmod/$n && svn up -r $pkgver)
    else
      svn co $_svntrunk$n/ --config-dir ./ -r $pkgver $_svnmod/$n
    fi
  done
  msg "SVN checkout done (or server timeout)"
}

package() {
  prepare #This must be here to set _svnmod correctly...
  #copy source files
  mkdir -p "$pkgdir/usr/share/upp"
  msg2 "Copying the source codes..."
  cp -r "$_svnmod/"{bazaar,examples,reference,tutorial,uppsrc} "$pkgdir/usr/share/upp/"
  echo "#define IDE_VERSION \"$pkgver-Arch-$(uname -m)\"" > "$pkgdir/usr/share/upp/uppsrc/ide/version.h"
  msg2 "Removing the .svn directories..."
  find "$pkgdir/" -type d -name ".svn" -exec rm -rf {} \; -prune
  #license
  mkdir -p "$pkgdir/usr/share/licenses/upp-svn"
  cp "$srcdir/license.txt" "$pkgdir/usr/share/licenses/upp-svn"
  #fix permissions
  msg2 "Setting permissions..."
  find "$pkgdir/usr/" -type f -exec chown root:root {} \; -exec chmod 644 {} \;
}


md5sums=('4a057aeaa906c8486cf463558b9a7d5a'
         'b214709f096e4f50d61f50988359241e')
