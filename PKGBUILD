pkgname=kanata
pkgver=1.8.1
pkgrel=1
pkgdesc="Improve keyboard comfort and usability with advanced customization"
arch=('x86_64')
url="https://github.com/jtroo/kanata"
license=('LGPL-3.0-only')
depends=(gcc-libs glibc)
makedepends=(cargo)
source=(
    kanata@.service
    kanata.sysusers
    99-kanata.rules
    "$pkgname-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz"
)
b2sums=('4bb789433699d6a6e3dc6e39cbba29757fe67b487d45eee0b74074c9fe21bf01fef2cc5464d0276e6068f36fcefc66a4e8359ffa6273b255f7e62ae2254052d8'
        'a38f3fb563af897bc14212f30a97dd791578e329b53061a0c845bebc73989b14e27a5882011cf8ed2f50504976859035479e1792b8f5c641dae9b0de5caf5813'
        '1f71a937a3afcab75e0a37b180fa765f18e572a505d4997f004af9b6b64712983290a04e51a12400bc20b07c8aceafb039da7f9e85e5e9174579d676a703db00'
        '58d20242e88598eb49e3efcd317cb9087ff3186fd78cd51f37fcadf118e84f70dcb84bf1273df724114184f21015974b9690182331ae24d2962ad40487f2ee63')
validpgpkeys=()

prepare() {
    cd "$pkgname-$pkgver"
    export RUSTUP_TOOLCHAIN=stable
    cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
    cd "$pkgname-$pkgver"
    export RUSTUP_TOOLCHAIN=stable
    export CARGO_TARGET_DIR=target
    export RUSTFLAGS="${RUSTFLAGS} --remap-path-prefix $srcdir=src"
    cargo build --frozen --release #--features cmd
}

check() {
    cd "$pkgname-$pkgver"
    export RUSTUP_TOOLCHAIN=stable
    cargo test --frozen --workspace #--features cmd
}

package() {
    cd "$pkgname-$pkgver"
    install -Dm0755 "target/release/$pkgname" -t "$pkgdir/usr/bin/"
    install -Dm0644 ../kanata@.service -t "$pkgdir/usr/lib/systemd/system/"
    install -Dm0644 ../kanata.sysusers "$pkgdir/usr/lib/sysusers.d/kanata.conf"
    install -Dm0644 ../99-kanata.rules "$pkgdir/usr/lib/udev/rules.d/99-kanata.rules"
}
