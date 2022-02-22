# Maintainer: Andrew Crerar <crerar@archlinux.org>
# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Jan-Erik Rediger <badboy at archlinux dot us>
# Contributor: nofxx <x@<nick>.com>

pkgname=redis
pkgver=6.2.6
pkgrel=2
pkgdesc='An in-memory database that persists on disk'
arch=('x86_64')
url='https://redis.io/'
license=('BSD')
depends=('jemalloc' 'grep' 'shadow' 'systemd-libs')
# pkg-config fails to detect systemd libraries if systemd is not installed
makedepends=('systemd' 'openssl')
backup=('etc/redis/redis.conf'
        'etc/redis/sentinel.conf')
install=redis.install
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/redis/redis/archive/${pkgver}.tar.gz"
        redis.service
        redis-sentinel.service
        redis.sysusers
        redis.tmpfiles
        redis.conf-sane-defaults.patch
        redis-5.0-use-system-jemalloc.patch)
sha512sums=('33dccbc511916ceb6793b0ba74f9639a2b3cf90334dc4a44a78256aa651cd60d12f848ab425f2ca6c64c138159657c3eadc600f49ce2dc6a8b444943491706c1'
            '8abf27f173a4532295dafd91b8e6e226e4376f1b2543c911e4fa60466d50523ada4dcfe520a738fd756c5725f4319153a0c0b26f6cdba234775114c72e4e7865'
            '2314c26920f5f0989fb98622f594b621a0b5035525146263da3fdfe640257118e03fc1903c15a62bcd4fbf260e0dcbf9249088292323739a607a11c9630795bf'
            '2227dfb41bf5112f91716f011862ba5fade220aea3b6a8134a5a05ee3af6d1cca05b08d793a486be97df98780bf43ac5dc4e5e9989ae0c5cd4e1eedb6cee5d71'
            '6a817024df70213205159de8350c684684d7dbda568c35a0a3d654bab0b91ec62d60b1a2aac6e1ffbe24040df4033b37a77361357834c572759f2d3c76d16ac0'
            'f45b5d20769159faeeb705e1bb9e4fdc3d74c0779b476cada829bfb49014c6ba6cd78d1d2751bf39acb6db4528281e9cab3aca684cadf687eb5fad10c7453154'
            '0acb08a6e0eaba239db7461bcfeddfbe0c1aaa517dc33c3918c9e991a1d5067cfe135b7f75085caade8c3ababd51ec9cefcc4120f57818bea1f7029a548a7732')

prepare() {
  cd $pkgname-$pkgver
  patch -Np1 < ../redis.conf-sane-defaults.patch
  patch -Np1 < ../redis-5.0-use-system-jemalloc.patch
}

build() {
  make BUILD_TLS=yes \
       USE_SYSTEMD=yes \
       -C $pkgname-$pkgver
}

package() {
  cd $pkgname-$pkgver
  make PREFIX="$pkgdir"/usr install

  install -Dm644 COPYING "$pkgdir"/usr/share/licenses/redis/LICENSE
  install -Dm644 -t "$pkgdir"/etc/redis redis.conf sentinel.conf
  install -Dm644 -t "$pkgdir"/usr/lib/systemd/system/ ../redis.service ../redis-sentinel.service
  install -Dm644 "$srcdir"/redis.sysusers "$pkgdir"/usr/lib/sysusers.d/redis.conf
  install -Dm644 "$srcdir"/redis.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/redis.conf
}
