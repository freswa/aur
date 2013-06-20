# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=2.6.14
pkgrel=1
pkgdesc="Advanced key-value store"
arch=('i686' 'x86_64')
url="http://redis.io/"
license=('BSD')
depends=('bash')
makedepends=('gcc>=3.1' 'make' 'pkgconfig')
backup=("etc/redis.conf"
	"etc/logrotate.d/redis")
source=("http://redis.googlecode.com/files/${pkgname}-${pkgver}.tar.gz"
	"redis.service"
	"redis.logrotate")
md5sums=('02e0c06e953413017ff64862953e2756'
         '5ab9fdb200e15c13b450fda77fa030b6'
         '9e2d75b7a9dc421122d673fe520ef17f')

build() {
  cd "$srcdir/${pkgname}-${pkgver}"
  make MALLOC=libc
  sed -i 's|# bind 127.0.0.1|bind 127.0.0.1|' redis.conf
}

package() {
  cd "$srcdir/${pkgname}-${pkgver}"
  mkdir -p $pkgdir/usr/bin
  make INSTALL_BIN="$pkgdir/usr/bin" PREFIX=/usr install

  install -D -m755 "$srcdir/${pkgname}-${pkgver}/COPYING" "$pkgdir/usr/share/licenses/redis/COPYING"
  install -Dm644 "$srcdir"/redis.service "$pkgdir"/usr/lib/systemd/system/redis.service
  install -Dm644 "$srcdir/redis.logrotate" "$pkgdir/etc/logrotate.d/redis"
  sed -i 's|daemonize no|daemonize yes|;s|dir \./|dir /var/lib/redis/|;s|logfile stdout|logfile /var/log/redis.log| ' $srcdir/${pkgname}-${pkgver}/redis.conf
  install -D -m644 "$srcdir/${pkgname}-${pkgver}/redis.conf" "$pkgdir/etc/redis.conf"
}
