pkgname=zig-git
pkgver=0.11.0.r2312.g2e7d28dd0d
pkgrel=1
pkgdesc="a programming language prioritizing robustness, optimality, and clarity"
arch=('x86_64')
url='https://ziglang.org'
license=('MIT')
makedepends=('cmake' 'git' 'ninja')
provides=(zig)
conflicts=(zig)
_depname="zig+llvm+lld+clang-$arch-linux-musl-0.12.0-dev.203+d3bc1cfc4"
source=(
	"git+https://github.com/ziglang/zig.git"
	"https://ziglang.org/deps/$_depname.tar.xz"
)
sha256sums=(
	'SKIP'
	'53f8af91937c9007ab00815fd4aac1367255a75e5824d03c0574193609d52a4a'
)

pkgver() {
    git -C zig describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    export ZIG="$srcdir/$_depname/bin/zig"
    export CC="$ZIG cc -target $arch-linux-musl -mcpu=baseline"
    export CXX="$ZIG c++ -target $arch-linux-musl -mcpu=baseline"
    export ZIG_GLOBAL_CACHE_DIR="$srcdir/build/zig-global-cache"
    export ZIG_LOCAL_CACHE_DIR="$srcdir/build/zig-local-cache"

    cmake -B build -S zig \
        -DCMAKE_INSTALL_PREFIX="/usr" \
        -DCMAKE_PREFIX_PATH="$srcdir/$_depname" \
        -DCMAKE_BUILD_TYPE=Release \
        -DZIG_TARGET_TRIPLE="$arch-linux-musl" \
        -DZIG_TARGET_MCPU="baseline" \
        -DZIG_STATIC=ON \
        -GNinja
}

build() {
    ninja -C build
}

# No `check()` because zig currently doesn't have a test target. See:
# https://github.com/ziglang/zig/issues/14240
# NOTE: In the future, a check step will be provided, but will likely be slow. Use `makepkg --nocheck` to skip it.

package() {
    DESTDIR="$pkgdir" ninja -C build install
}
