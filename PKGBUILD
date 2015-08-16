# custom variables
_hkgname=hledger-lib
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-hledger-lib
pkgver=0.14
pkgrel=20
pkgdesc="Reusable types and utilities for the hledger accounting tool and financial apps in general."
url="http://hledger.org"
license=("GPL")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.0.3-2"
         "sh"
         "haskell-hunit=1.2.2.3"
         "haskell-bytestring=0.9.1.10"
         "haskell-containers=0.4.0.0"
         "haskell-directory=1.1.0.0"
         "haskell-filepath=1.2.0.0"
         "haskell-mtl=2.0.1.0"
         "haskell-old-locale=1.0.0.2"
         "haskell-old-time=1.0.0.6"
         "haskell-parsec=3.1.1"
         "haskell-regexpr=0.5.4"
         "haskell-safe=0.3"
         "haskell-split=0.1.4.1"
         "haskell-time=1.2.0.3"
         "haskell-utf8-string=0.3.6")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=("9a6d6ab4383800279e135e9bbcd886e95cea45232d093202c5d43e6edd1f927c")

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
