# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=2.6.6
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
	"redis.d"
	"redis.service"
	"redis.logrotate")
md5sums=('ebba5449b5d57f258386dbeab2cffdf9'
         '8d843919d9f165e9a47e56cadb4ac2ed'
         '5ab9fdb200e15c13b450fda77fa030b6'
         '9e2d75b7a9dc421122d673fe520ef17f')

build() {
  cd "$srcdir/${pkgname}-${pkgver}"
  make MALLOC=libc
}

package() {
  cd "$srcdir/${pkgname}-${pkgver}"
  mkdir -p $pkgdir/usr/bin
  make INSTALL_BIN="$pkgdir/usr/bin" PREFIX=/usr install

  install -D -m755 "$srcdir/${pkgname}-${pkgver}/COPYING" "$pkgdir/usr/share/licenses/redis/COPYING"
  install -D -m755 "$srcdir/redis.d" "$pkgdir/etc/rc.d/redis"
  install -Dm644 "$srcdir"/redis.service "$pkgdir"/usr/lib/systemd/system/redis.service
  install -Dm644 "$srcdir/redis.logrotate" "$pkgdir/etc/logrotate.d/redis"
  sed -i 's|daemonize no|daemonize yes|;s|dir \./|dir /var/lib/redis/|;s|logfile stdout|logfile /var/log/redis.log| ' $srcdir/${pkgname}-${pkgver}/redis.conf
  install -D -m644 "$srcdir/${pkgname}-${pkgver}/redis.conf" "$pkgdir/etc/redis.conf"
}
