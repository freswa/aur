# Maintainer:  Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer:  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=3.0.2
pkgrel=1
pkgdesc='Advanced key-value store'
arch=('i686' 'x86_64')
url='http://redis.io/'
license=('BSD')
depends=('jemalloc' 'grep' 'shadow')
backup=('etc/redis.conf'
        'etc/logrotate.d/redis')
install=redis.install
source=(http://download.redis.io/releases/redis-$pkgver.tar.gz
        redis.service
        redis.logrotate
        redis.conf-sane-defaults.patch
        redis-2.8.11-use-system-jemalloc.patch)
md5sums=('87be8867447f62524b584813e5a7bd14'
         'afa0f4a9000c26c3323e83fe34883e23'
         '9e2d75b7a9dc421122d673fe520ef17f'
         'b1beae6954b780da261b4056fd7f918a'
         '2ae176578f538e37a67a463258302bc6')

prepare() {
  cd $pkgname-$pkgver
  patch -p1 -i ../redis.conf-sane-defaults.patch
  patch -p1 -i ../redis-2.8.11-use-system-jemalloc.patch
}

build() {
  make -C $pkgname-$pkgver
}

package() {
  cd $pkgname-$pkgver
  make PREFIX="$pkgdir"/usr install

  install -Dm644 COPYING "$pkgdir"/usr/share/licenses/redis/LICENSE
  install -Dm644 redis.conf "$pkgdir"/etc/redis.conf
  install -Dm644 ../redis.service "$pkgdir"/usr/lib/systemd/system/redis.service

  # files kept for compatibility with installations made before 2.8.13-2
  install -Dm644 ../redis.logrotate "$pkgdir"/etc/logrotate.d/redis

  ln -sf redis-server $pkgdir/usr/bin/redis-sentinel
}
