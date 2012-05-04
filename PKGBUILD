# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=2.4.13
pkgrel=1
pkgdesc="Advanced key-value store"
arch=('i686' 'x86_64')
url="http://redis.io/"
#url="http://code.google.com/p/redis"
license=('BSD')
depends=('bash')
makedepends=('gcc>=3.1' 'make' 'pkgconfig')
backup=("etc/redis.conf"
	"etc/logrotate.d/redis")
source=("http://redis.googlecode.com/files/${pkgname}-${pkgver}.tar.gz"
	"redis.d"
	"redis.logrotate")
md5sums=('f8a160f8a6fe7d7e8ebb4c23ff583d71'
         '4f2c02b481283d1336508c988e60e8d8'
         '9e2d75b7a9dc421122d673fe520ef17f')

build() {
    cd "$srcdir/${pkgname}-${pkgver}"
    CFLAGS="$CFLAGS -std=c99" make FORCE_LIBC_MALLOC=yes
}

package() {
    cd "$srcdir/${pkgname}-${pkgver}"
    mkdir -p $pkgdir/usr/bin
    make INSTALL_BIN="$pkgdir/usr/bin" PREFIX=/usr install

    install -D -m755 "$srcdir/${pkgname}-${pkgver}/COPYING" "$pkgdir/usr/share/licenses/redis/COPYING"
    install -D -m755 "$srcdir/redis.d" "$pkgdir/etc/rc.d/redis"
    install -Dm644 "$srcdir/redis.logrotate" "$pkgdir/etc/logrotate.d/redis"
    sed -i 's|daemonize no|daemonize yes|;s|dir \./|dir /var/lib/redis/|;s|logfile stdout|logfile /var/log/redis.log| ' $srcdir/${pkgname}-${pkgver}/redis.conf
    install -D -m644 "$srcdir/${pkgname}-${pkgver}/redis.conf" "$pkgdir/etc/redis.conf"
}
