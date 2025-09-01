pkgname=ruby
pkgver=3.4.5
pkgrel=1
pkgdesc="An object-oriented language for quick and easy programming"
arch=('x86_64')
url="https://www.ruby-lang.org/en"
license=('BSD-2-Clause')
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
options=('!emptydirs')
source=(https://cache.ruby-lang.org/pub/ruby/${pkgver%.*}/${pkgname}-${pkgver}.tar.xz)
sha256sums=(7b3a905b84b8777aa29f557bada695c3ce108390657e614d2cc9e2fb7e459536)

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --disable-rpath
        --enable-shared
        --without-baseruby
        ac_cv_func_qsort_r=no
        --with-dbm-type=gdbm_compat
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
        ${configure_options}
    )

    export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
    export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

    ./configure "${configure_args[@]}"

    make
    make capi
}

package() {
    cd ${pkgname}-${pkgver}

    make DESTDIR=${pkgdir} install
}
