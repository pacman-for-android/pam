# Maintainer: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: judd <jvinet@zeroflux.org>

pkgname=pam
pkgver=1.1.1
pkgrel=2
pkgdesc="PAM (Pluggable Authentication Modules) library"
arch=('i686' 'x86_64')
license=('GPL2')
url="http://www.kernel.org/pub/linux/libs/pam/"
depends=('glibc' 'db' 'cracklib')
makedepends=('flex')
backup=(etc/security/{access.conf,group.conf,limits.conf,namespace.conf,namespace.init,pam_env.conf,time.conf} etc/pam.d/other etc/default/passwd etc/environment)
source=(http://www.kernel.org/pub/linux/libs/pam/library/Linux-PAM-$pkgver.tar.bz2
        ftp://ftp.suse.com/pub/people/kukuk/pam/pam_unix2/pam_unix2-2.6.tar.bz2
        pam-1.1.1-db5.patch
        other)
options=('!libtool' '!emptydirs')
md5sums=('9b3d952b173d5b9836cbc7e8de108bee'
         'e2788389a6c59224110a45fcff30e02b'
         '7ab50aacef6871dcf900a1d4df158667'
         '6e6c8719e5989d976a14610f340bd33a')

build() {
  cd $srcdir/Linux-PAM-$pkgver
  patch -Np1 -i $srcdir/pam-1.1.1-db5.patch
  ./configure --sysconfdir=/etc DESTDIR=$pkgdir --libdir=/lib
  make
  make INSTALL=/bin/install DESTDIR=$pkgdir install
  install -D -m644 ../other $pkgdir/etc/pam.d/other
  # build pam_unix2 module
  # source ftp://ftp.suse.com/pub/people/kukuk/pam/pam_unix2
  cd $srcdir/pam_unix2-2.6
  ./configure
  make
  make DESTDIR=$pkgdir install
  # add the realtime permissions for audio users
  sed -i 's|# End of file||' $pkgdir/etc/security/limits.conf
  cat >>$pkgdir/etc/security/limits.conf <<_EOT
*               -       rtprio          0
*               -       nice            0
@audio          -       rtprio          65
@audio          -       nice           -10
@audio          -       memlock         40000
_EOT
  # fix some missing symlinks from old pam for compatibility
  cd $pkgdir/lib/security
  ln -s pam_unix.so pam_unix_acct.so
  ln -s pam_unix.so pam_unix_auth.so
  ln -s pam_unix.so pam_unix_passwd.so
  ln -s pam_unix.so pam_unix_session.so
  # set unix_chkpwd uid
  chmod +s $pkgdir/sbin/unix_chkpwd
}
