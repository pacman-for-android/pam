# $Id: PKGBUILD,v 1.34 2007/11/16 00:02:36 daniel Exp $
# Maintainer: judd <jvinet@zeroflux.org>
pkgname=pam
pkgver=0.99.9.0
pkgrel=2
pkgdesc="PAM (Pluggable Authentication Modules) library"
arch=(i686 x86_64)
license=('GPL2')
url="http://www.kernel.org/pub/linux/libs/pam/"
groups=('base')
depends=('glibc' 'db>=4.6' 'cracklib')
backup=(etc/security/{access.conf,group.conf,limits.conf,namespace.conf,namespace.init,pam_env.conf,time.conf} etc/pam.d/other etc/default/passwd etc/environment)
source=(http://www.kernel.org/pub/linux/libs/pam/pre/library/Linux-PAM-$pkgver.tar.bz2 \
        ftp://ftp.suse.com/pub/people/kukuk/pam/pam_unix2/pam_unix2-2.1.tar.bz2 \
        other)
options=('!libtool' '!emptydirs')
md5sums=('f526c794482ce21c31866549e05c45de'
         '08d3bc1940897b5dfcbe2f51dd979ad0'
	 '6e6c8719e5989d976a14610f340bd33a')

build() {
  export MAKEFLAGS="-1"
  cd $startdir/src/Linux-PAM-$pkgver
  ./configure --sysconfdir=/etc DESTDIR=$startdir/pkg --libdir=/lib
  make || return 1
  make INSTALL=/bin/install DESTDIR=$startdir/pkg install
  install -D -m644 ../other $startdir/pkg/etc/pam.d/other
  # build pam_unix2 module
  # source ftp://ftp.suse.com/pub/people/kukuk/pam/pam_unix2
  cd $startdir/src/pam_unix2-2.1
  ./configure
  make || return 1
  make DESTDIR=$startdir/pkg install
  # add the realtime permissions for audio users
  sed -i 's|# End of file||' $startdir/pkg/etc/security/limits.conf
  cat >>$startdir/pkg/etc/security/limits.conf <<_EOT
*               -       rtprio          0
*               -       nice            0
@audio          -       rtprio          65
@audio          -       nice           -10
@audio          -       memlock         40000
_EOT
  # fix some missing symlinks from old pam for compatibility
  cd $startdir/pkg/lib/security
  ln -s pam_unix.so pam_unix_acct.so
  ln -s pam_unix.so pam_unix_auth.so
  ln -s pam_unix.so pam_unix_passwd.so
  ln -s pam_unix.so pam_unix_session.so
  # set unix_chkpwd uid
  chmod +s $startdir/pkg/sbin/unix_chkpwd
}
