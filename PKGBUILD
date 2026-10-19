# Maintainer: Masahiro Sugaya <xsigil@section-9.sh>
pkgname=airdrop-zero-git
pkgver=0.1.0.r0.g1234567
pkgrel=1
pkgdesc="Minimal zero-cloud, air-gapped local file ingestion primitive"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://github.com/xsigil/airdrop-zero"
license=('MIT')
depends=('glibc')
makedepends=('git' 'go')
provides=('airdrop-zero')
conflicts=('airdrop-zero')
source=("git+https://github.com/xsigil/airdrop-zero.git")
sha256sums=('SKIP')

pkgver() {
  cd "${srcdir}/airdrop-zero"
  git describe --long --tags --abbrev=7 2>/dev/null | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g' ||
  printf "0.1.0.r%s.g%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
}

build() {
  cd "${srcdir}/airdrop-zero"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external"

  go build -ldflags="-s -w" -o airdrop-zero main.go
}

package() {
  cd "${srcdir}/airdrop-zero"
  install -Dm755 airdrop-zero "${pkgdir}/usr/bin/airdrop-zero"
  if [ -f LICENSE ]; then
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  fi
}
