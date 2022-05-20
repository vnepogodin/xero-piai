# Maintainer: DarkXero <info@techxero.com>
# Contributor: Vladislav Nepogodin <nepogodin.vlad@gmail.com>

pkgname=xerolinux-tool
_destname1="/"
_pkgverpkginst=1.0.5
_urlpkginst="https://github.com/xerolinux/xero-piai"
pkgver=0.2.6
pkgrel=1
pkgdesc="XeroLinux Configuration Tool"
arch=('any')
url="https://github.com/XeroLinux"
license=('GPL3')
makedepends=('cmake' 'ninja' 'git' 'polkit-qt5')
depends=('qt5-base')
replaces=('sysconfig')
provides=("${pkgname}")
options=(!strip !emptydirs)
source=(${pkgname}::"git+${url}/${pkgname}")
source+=("xero-piai::git+$_urlpkginst")
sha256sums=('SKIP'
            'e78286cdbd687260762be8ec9488436c800544db608cd7c787c349d1d96ab9c3')

build() {
  cd ${srcdir}/xero-piai

  _cpuCount=$(grep -c -w ^processor /proc/cpuinfo)

  sed -i -e "s/CachyOS/XeroLinux/g" src/main.cpp
  sed -i -e "s/cachyos/xerolinux/g" src/main.cpp
  sed -i -e "s/cachyospi/xeropiai/g" src/main.cpp
  sed -i -e "s/CachyOS/XeroLinux/g" src/mainwindow.cpp
  sed -i -e "s/XeroLinux Setup Assistant/XeroLinux Post install App Installer/g" src/mainwindow.cpp
  sed -i -e "s/cachyospi/xeropiai/g" src/mainwindow.cpp
  sed -i -e "s/cachyos-pi/xero-piai/g" src/mainwindow.cpp

  cmake -S . -Bbuild \
        -GNinja \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=lib
  cmake --build build --parallel $_cpuCount
}

package() {
	install -dm755 ${pkgdir}${_destname1}
	cp -r ${srcdir}/${pkgname}${_destname1}/* ${pkgdir}${_destname1}
	rm "${pkgdir}${_destname1}/push.sh"
	rm "${pkgdir}${_destname1}/README.md"
	rm "${pkgdir}${_destname1}/PKGBUILD"
	rm "${pkgdir}${_destname1}/LICENSE"

    cd ${srcdir}/xero-piai
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/cachyos-packageinstaller/LICENSE"
    cd build
    mv cachyos-pi-bin xero-piai
    install -Dm744 xero-piai "${pkgdir}/usr/lib/xero-piai/xero-piai"
}
