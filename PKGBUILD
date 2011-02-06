# Contributor: Jens Pranaitis <jens@chaox.net>
pkgname=airoscript-svn
pkgver=1565
pkgrel=2
pkgdesc="script to simplify the use of aircrack-ng tools"
arch=("i686" "x86_64")
url="http://code.google.com/p/airoscript/"
license=('GPL')
depends=("bash" "gettext" "perl" "aircrack-ng-svn" "screen" "pptpclient")
makedepends=('subversion')
source=("airoscript-conf.patch")
md5sums=('3f9d1bfb65560ab1af08ee0bac8cecbc')
_svntrunk=http://trac.aircrack-ng.org/svn/branch/airoscript/
_svnmod=airoscript
build() {
  cd "$srcdir"
 
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi
  msg "SVN checkout done or server timeout"
  msg "Starting make..."
  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"
	#some things needs path fix
  cd src/conffiles
  patch -Np0 -i "$srcdir"/airoscript-conf.patch || return 1
  cd ../..
  #we must fix paths in Makefiles, /usr/local is too bad, this dir must be banned
	sed -i "s|/usr/local|$pkgdir/usr|" Makefile || return 1
  sed -i 's|$(prefix)/etc|$(prefix)/../etc|' Makefile-Linux || return 1
  #installation
	mkdir -p $pkgdir/usr/sbin
  make DESTDIR="$pkgdir/" install || return 1
  # renaming screenrc or we will be in conflict hell
  cd "$pkgdir"/etc
  mv screenrc screenrc-airoscript
  # why the fuck?
  rm "$pkgdir"/usr/share/locale/es_ES
}
