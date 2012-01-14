# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=2.4.6
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
md5sums=('41d394074bcde762872ecb5506f35aee'
         '9726d06d0a0c60cb5d55a31b3dc1e55d'
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
