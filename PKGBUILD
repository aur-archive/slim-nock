# $Id: PKGBUILD 163488 2012-07-13 10:14:32Z tpowa $
# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Thayer Williams <thayer@archlinux.org>
# Contributor: Alexander Fehr <pizzapunk gmail com>
# Contributor: Hugo Ideler <hugoideler@dse.nl>
#	AUR Package Maintainer: Diogo Bento <db_eee-at-hotmail-dot-com>

pkgname=slim-nock
_pkgname=slim
pkgver=1.3.4
pkgrel=2
pkgdesc='Desktop-independent graphical login manager for X11. Without Consolekit'
arch=('i686' 'x86_64')
url='http://slim.berlios.de/'
license=('GPL2')
depends=('pam' 'libxmu' 'libpng' 'libjpeg' 'libxft')
makedepends=('cmake' 'freeglut')
provides=("slim=${pkgver}")
conflicts=('slim')
backup=('etc/slim.conf' 'etc/logrotate.d/slim' 'etc/pam.d/slim')
source=("http://download.berlios.de/${_pkgname}/${_pkgname}-${pkgver}.tar.gz"
        'rc.d'
        'pam.d'
        'logrotate'
        'slim.service'
        'session-name.patch'
        'libpng-1.4+-support.patch')

install=install

build() {
        cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p1 -i ../session-name.patch # FS#26693: fix default session name
        patch -Np1 -i ../libpng-1.4+-support.patch # taken from gentoo to build
        cd ${srcdir}
        mkdir build
        cd build
        cmake ../${_pkgname}-${pkgver} \
                -DCMAKE_BUILD_TYPE=Release \
                -DCMAKE_SKIP_RPATH=ON \
                -DCMAKE_INSTALL_PREFIX=/usr \
                -DUSE_PAM=yes -DUSE_CONSOLEKIT=no
	make
}

package() {
        cd  ${srcdir}/build/
	make DESTDIR="${pkgdir}" install

	install -D -m755 ../rc.d "${pkgdir}"/etc/rc.d/slim
	install -D -m644 ../pam.d "${pkgdir}"/etc/pam.d/slim
	install -D -m644 ../logrotate "${pkgdir}"/etc/logrotate.d/slim

	# Provide sane defaults
	sed -i 's|#xserver_arguments.*|xserver_arguments -nolisten tcp vt07|' "${pkgdir}"/etc/slim.conf
	sed -i 's|/var/run/slim.lock|/var/lock/slim.lock|' "${pkgdir}"/etc/slim.conf
        # install systemd files
        install -D -m644 ${srcdir}/slim.service ${pkgdir}/usr/lib/systemd/system/slim.service
}
md5sums=('51543533e492b41007811f7d880720fa'
         'd8ea9c4dee2811524b67f4f666311a1f'
         'd33edc74724c6ca00445767ce38fc732'
         '43da096480bf72c3ccec8ad8400f34f0'
         'a5d6bde9e63899df7d2081e1585bbe54'
         'ebcb6829028615686de7b64ceeaaf8ed'
         '6d19bd7a91592ed2bb902b22b9594565')
