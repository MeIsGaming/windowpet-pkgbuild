pkgname=windowpet-git
pkgver=r101.aba8d3a
pkgrel=1
pkgdesc="WindowPet desktop overlay application (Git Version)"
arch=('x86_64')
url="https://github.com/SeakMengs/WindowPet"
license=('MIT')
depends=('sfml2' 'libx11' 'libxrandr' 'nodejs' 'yarn')
makedepends=('git' 'cmake' 'nodejs' 'yarn' 'rustup' 'rust' 'cargo')
provides=('windowpet')
conflicts=('windowpet')

source=("git+https://github.com/SeakMengs/WindowPet.git")
sha256sums=('SKIP')

pkgver() {
  cd "$srcdir/WindowPet"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "$srcdir/WindowPet"

  # Node/Yarn Dependencies
  yarn install --frozen-lockfile --network-concurrency 16 --prefer-offline


  # Build frontend + Rust (Tauri)
  export CARGO_BUILD_JOBS=$(nproc)
  rustup default stable
  yarn tauri build
}

package() {
  cd "$srcdir/WindowPet"

  # Install app binary (Tauri Linux build usually under src-tauri/target/release/)
  install -Dm755 "src-tauri/target/release/windowpet" "$pkgdir/usr/bin/windowpet"

  # Optional: include desktop file / icons if vorhanden
  install -Dm644 windowpet.desktop "$pkgdir/usr/share/applications/windowpet.desktop"
  install -Dm644 assets/icon.png "$pkgdir/usr/share/icons/hicolor/512x512/apps/windowpet.png"
}
