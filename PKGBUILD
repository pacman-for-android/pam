# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=pam
pkgver=1.5.3
pkgrel=3.6
pkgdesc="PAM (Pluggable Authentication Modules) library"
arch=('x86_64' 'aarch64')
license=('GPL2')
url="http://linux-pam.org"
depends=('glibc' 'libtirpc' 'pambase' 'audit' 'libaudit.so' 'libxcrypt' 'libcrypt.so')
makedepends=('flex' 'docbook-xml>=4.4' 'docbook-xsl')
provides=('libpam.so' 'libpamc.so' 'libpam_misc.so')
backup=(data/etc/security/{access.conf,faillock.conf,group.conf,limits.conf,namespace.conf,namespace.init,pwhistory.conf,pam_env.conf,time.conf} etc/environment)
source=(https://github.com/linux-pam/linux-pam/releases/download/v$pkgver/Linux-PAM-$pkgver{,-docs}.tar.xz{,.asc}
        $pkgname.tmpfiles
        pam-change-prefix.patch)
validpgpkeys=(
        '8C6BFD92EE0F42EDF91A6A736D1A7F052E5924BB' # Thorsten Kukuk
        '296D6F29A020808E8717A8842DB5BD89A340AEB7' #Dimitry V. Levin <ldv@altlinux.org>
)

sha256sums=('7ac4b50feee004a9fa88f1dfd2d2fa738a82896763050cd773b3c54b0a818283'
            'SKIP'
            'fe7493aa0a281f8cfe81814768329f953098d0fd8073da1dc0bd64494d022d4d'
            'SKIP'
            '5b55e472725088f743e24424ea365e16d96c921149309be41826916d8d037357'
            'cc0382b570cb813f8ece45b2beba52f7ffa4dd5cb6dd9d290d97a6cd67cb889c')

options=('!emptydirs')

prepare() {
  cd Linux-PAM-$pkgver
  patch -Np2 -i ../pam-change-prefix.patch
}

build() {
  cd Linux-PAM-$pkgver
  ./configure \
    --prefix=/data/usr \
    --libdir=/data/usr/lib \
    --sbindir=/data/usr/bin \
    --sysconfdir=/data/etc \
    --includedir=/data/usr/include/security \
    --enable-logind \
    --disable-db
  make
}

package() {
  install -Dm 644 $pkgname.tmpfiles "$pkgdir"/data/usr/lib/tmpfiles.d/$pkgname.conf
  cd Linux-PAM-$pkgver
  make DESTDIR="$pkgdir" SCONFIGDIR=/data/etc/security SHELL=/data/usr/bin/sh install

  # set unix_chkpwd uid
  chmod +s "$pkgdir"/data/usr/bin/unix_chkpwd
}

# vim: ts=2 sw=2 et:
