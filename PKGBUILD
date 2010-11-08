# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=2.0.4
pkgrel=1
pkgdesc="Advanced key-value store"
arch=('i686' 'x86_64')
url="http://code.google.com/p/redis"
license=('BSD')
depends=('bash')
makedepends=('gcc>=3.1' 'make' 'pkgconfig')
source=("http://redis.googlecode.com/files/${pkgname}-${pkgver}.tar.gz"
        "redis.d")
md5sums=('60656113efb6759ab644d7495629d90b'
         '7de0d0e70cef571a2da9a431e4b40917')

build() {
    cd "$srcdir/${pkgname}-${pkgver}"
    CFLAGS="$CFLAGS -std=c99" make

    sed -i 's|daemonize no|daemonize yes|;s|dir \./|dir /var/lib/redis/|;s|logfile stdout|logfile /var/log/redis.log| ' $srcdir/${pkgname}-${pkgver}/redis.conf

    install -D -m755 "$srcdir/${pkgname}-${pkgver}/redis-server" "$pkgdir/usr/bin/redis-server"
    install -D -m755 "$srcdir/${pkgname}-${pkgver}/redis-cli" "$pkgdir/usr/bin/redis-cli"
    install -D -m755 "$srcdir/${pkgname}-${pkgver}/redis-benchmark" "$pkgdir/usr/bin/redis-benchmark"

    install -D -m755 "$srcdir/${pkgname}-${pkgver}/COPYING" "$pkgdir/usr/share/licenses/redis/COPYING"

    install -D -m755 "$srcdir/${pkgname}-${pkgver}/redis.conf" "$pkgdir/etc/redis.conf"
    install -D -m755 "$srcdir/redis.d" "$pkgdir/etc/rc.d/redis"
}
