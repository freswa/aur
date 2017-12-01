# Maintainer:  Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer:  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=4.0.4
pkgrel=1
pkgdesc='Advanced key-value store'
arch=('x86_64')
url='http://redis.io/'
license=('BSD')
depends=('jemalloc' 'grep' 'shadow')
backup=('etc/redis.conf'
        'etc/logrotate.d/redis')
source=(http://download.redis.io/releases/redis-$pkgver.tar.gz
        redis.service redis.sysusers redis.tmpfiles
        redis.logrotate
        redis.conf-sane-defaults.patch
        redis-2.8.11-use-system-jemalloc.patch)
sha256sums=('35768145335e874b1b810e23494ad3daa6f442c3dc1d7e3784992ba50799c0cd'
            'cceff2a097d9041a0c73caeb5c33e849af783c6a12db866f24b8417ac3ac9d11'
            '78f6ab83408956a9afaf28689128f382545c901f172cd5b670724c73f6896d5d'
            '09dcc8522899dc3d1e9362989aa4acb5a3996d700b9a44f22ebb5b9667b88183'
            '8b4c2caabb4f54157ad91ca472423112b1803685ad18ed11b60463d78494df13'
            '22cd3b9f7e9b17647a615d009b50603e7978b0af26c3e2c53560e57573b996ed'
            '9720468ede366893c32f34616c6d8670e790309757ae0abc0f49402089a7a672')

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

  ln -sf redis-server "$pkgdir"/usr/bin/redis-sentinel

  install -Dm644 "$srcdir"/redis.sysusers "$pkgdir"/usr/lib/sysusers.d/redis.conf
  install -Dm644 "$srcdir"/redis.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/redis.conf
}
