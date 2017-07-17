# Maintainer:  Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer:  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=4.0.0
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
sha256sums=('d539ae309295721d5c3ed7298939645b6f86ab5d25fdf2a0352ab575c159df2d'
            'cceff2a097d9041a0c73caeb5c33e849af783c6a12db866f24b8417ac3ac9d11'
            '8b4c2caabb4f54157ad91ca472423112b1803685ad18ed11b60463d78494df13'
            '22cd3b9f7e9b17647a615d009b50603e7978b0af26c3e2c53560e57573b996ed'
            'b1d2802d7e459799565fb4660e97e57a23de0aa47689656ece4a00d1053dd919')

prepare() {
  cd $pkgname-$pkgver
  patch -p1 -i ../redis.conf-sane-defaults.patch
#  patch -p1 -i ../redis-2.8.11-use-system-jemalloc.patch
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

  ln -sf redis-server "$pkgdir"/usr/bin/redis-sentinel
}
