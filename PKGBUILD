pkgname=(
    ruby
    ruby-docs
)
pkgbase=ruby
pkgver=3.4.7
pkgrel=2
pkgdesc="An object-oriented language for quick and easy programming"
arch=('x86_64')
url="https://www.ruby-lang.org/en"
license=('BSD-2-Clause')
makedepends=(
    'doxygen'
    'gcc-libs'
    'gdbm'
    'glibc'
    'gmp'
    'graphviz'
    'libffi'
    'libxcrypt'
    'libyaml'
    'openssl'
    'readline'
    'rust'
    'tk'
    'zlib'
)
options=('!emptydirs')
source=(https://cache.ruby-lang.org/pub/ruby/${pkgver%.*}/${pkgbase}-${pkgver}.tar.xz
    https://github.com/ruby/openssl/commit/e8481cd687840f6d8247ca70967c1de47093ecb8.patch)
sha256sums=(db425a86f6e07546957578f4946cc700a91e7fd51115a86c56e096f30e0530c7
    ff983088dc531b2f4418196bb4cf14953f8b2efc67ab64921b9589d830514040)

prepare() {
    cd ${pkgbase}-${pkgver}

    (
        cd ext/openssl

        patch --verbose --strip=1 --input=${srcdir}/e8481cd687840f6d8247ca70967c1de47093ecb8.patch
    )

    autoreconf -fiv
}

build() {
    cd ${pkgbase}-${pkgver}

    local configure_args=(
        --sysconfdir=/etc
        --localstatedir=/var
        --sharedstatedir=/var/lib
        --enable-shared
        --disable-rpath
        --with-dbm-type=gdbm_compat
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
        ${configure_options}
    )

    # this uses malloc_usable_size, which is incompatible with fortification level 3
    export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
    export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

    ./configure "${configure_args[@]}"

    make
    make rdoc capi
}

package_ruby() {
    depends=(
        'gcc-libs'
        'gdbm'
        'glibc'
        'gmp'
        'libffi'
        'libxcrypt'
        'libyaml'
        'openssl'
        'readline'
        'zlib'
    )

    cd ${pkgbase}-${pkgver}

    make DESTDIR=${pkgdir} install-nodoc
}


package_ruby-docs() {
    pkgdesc="Documentation files for Ruby"

    cd ${pkgbase}-${pkgver}

    make DESTDIR=${pkgdir} install-doc install-capi
}
