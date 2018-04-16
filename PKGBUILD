# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=pam
pkgver=1.3.0
pkgrel=2
pkgdesc="PAM (Pluggable Authentication Modules) library"
arch=('x86_64')
license=('GPL2')
url="http://linux-pam.org"
depends=('glibc' 'cracklib' 'libtirpc' 'pambase')
makedepends=('flex' 'w3m' 'docbook-xml>=4.4' 'docbook-xsl')
backup=(etc/security/{access.conf,group.conf,limits.conf,namespace.conf,namespace.init,pam_env.conf,time.conf} etc/default/passwd etc/environment)
source=(http://linux-pam.org/library/Linux-PAM-$pkgver.tar.bz2)
md5sums=('da4b2289b7cfb19583d54e9eaaef1c3a')

options=('!emptydirs')

build() {
  cd $srcdir/Linux-PAM-$pkgver
  ./configure --libdir=/usr/lib --sbindir=/usr/bin --disable-db
  make
}

package() {
  cd $srcdir/Linux-PAM-$pkgver
  make DESTDIR=$pkgdir SCONFIGDIR=/etc/security install

  # set unix_chkpwd uid
  chmod +s $pkgdir/usr/bin/unix_chkpwd

  # remove doc which is not used anymore
  # FS #40749
  rm $pkgdir/usr/share/doc/Linux-PAM/sag-pam_userdb.html
}
