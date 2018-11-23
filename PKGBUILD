# Maintainer: Andrew Crerar <crerar@archlinux.org>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=5.0.2
pkgrel=1
pkgdesc='Advanced key-value store'
arch=('x86_64')
url='https://redis.io/'
license=('BSD')
depends=('jemalloc' 'grep' 'shadow')
backup=('etc/redis.conf'
        'etc/logrotate.d/redis')
source=(http://download.redis.io/releases/redis-$pkgver.tar.gz
        redis.service
        redis.sysusers
        redis.tmpfiles
        redis.logrotate
        redis.conf-sane-defaults.patch
        redis-5.0-use-system-jemalloc.patch)
sha512sums=('0af94e2a0390d99aa2290f6a211a1173c7f8f1e7843792d5177271bac1c432b135756b4e94437abd0ac85d18be5a7aa05ff931cc900348c7c9c7872d759de2ce'
            '86018ddd6625f918295e10f9478da361f73a6dbd6c6b8e4b974201669bcccbd4dba443bb0844be68f6ab8d5a1762b32af04c5e12df53b1f0ea812b790d9f4e37'
            '2227dfb41bf5112f91716f011862ba5fade220aea3b6a8134a5a05ee3af6d1cca05b08d793a486be97df98780bf43ac5dc4e5e9989ae0c5cd4e1eedb6cee5d71'
            '68f7bc12e3b95cb199b71255c6aa5bfaa431fbabbc7d2308e54347c0d35e6d8091c4a79a5a6b56494ab3a294f9389e3ec63902931920862f60b1ffe77222eeeb'
            'df11492df0458b224f75fff31475d39b85116cba6deb06d80d0fd8c467d221db51a2a8f5fc5d2e3e8239c0718e1cf5dc12e99cac9019cb99d3f11835ad00aa5d'
            'fe9748e0ab326e429f4183016b5aeb772199cd4688371c320811c25f8de2fcb7bc34955b359612c1a287e83b4afaba3b7fd6a6567fad66c04e8482cc802f3b50'
            '55b4085900c54fa7e7bf1c2bad7fba8cdbaf496a3f83b6d32fccb8aed5048cdde1690fea0485162dbb637e7fafb00a6b995908fa6db55e77854eb9f575b54d40')

prepare() {
  cd $pkgname-$pkgver
  patch -p1 -i ../redis.conf-sane-defaults.patch
  patch -p1 -i ../redis-5.0-use-system-jemalloc.patch
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
