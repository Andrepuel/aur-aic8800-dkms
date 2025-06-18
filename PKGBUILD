# Maintainer: Franck Duriez <franck@duriez.info>

pkgname=aic8800-dkms
pkgver=1.0.6
pkgrel=1

pkgdesc="Kernel modules for BrosTrend AX300 WiFi 6"
arch=('any')
url="https://www.brostrend.com/products/ax5l"
license=('GPLv2')

source=(
  'https://linux.brostrend.com/aic8800-dkms.deb'
  '0001-Make-CONFIG_RFTEST-n-valid.patch'
  '0002-Fix-DKMS-config.patch'
  '0003-Fix-kernel-logs.patch'
  '0004-fix-linux-6.13-build.patch'
)
sha512sums=(
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
)

prepare() {
  cd "${srcdir}"
  tar xvzf "data.tar.gz"
  cd "usr/src/aic8800-${pkgver}"
  patch -Np1 -i ../../../../0001-Make-CONFIG_RFTEST-n-valid.patch -d .
  patch -Np1 -i ../../../../0002-Fix-DKMS-config.patch -d .
  patch -Np1 -i ../../../../0003-Fix-kernel-logs.patch -d .
  patch -Np1 -i ../../../../0004-fix-linux-6.13-build.patch -d .
  # adapt for 6.15.2-arch1-1
  find . -name "*.c" -o -name "*.h" | xargs sed -i 's/del_timer/timer_delete/g' 
}

build() {
  cd "${srcdir}"
}

package() {
  # Copy udev rules
  install -dm 755 "${pkgdir}/usr/lib/udev/rules.d"
  install -m 644 "${srcdir}/lib/udev/rules.d/aic.rules" "${pkgdir}/usr/lib/udev/rules.d/"

  # Copy device firmware
  install -dm 755 "${pkgdir}/usr/lib/firmware"
  cp -dr --no-preserve=ownership "${srcdir}/lib/firmware/aic8800DC" "${pkgdir}/usr/lib/firmware"

  # Copy source and dkms config
  install -dm 755 "${pkgdir}/usr/src/"
  cp -dr --no-preserve=ownership "${srcdir}/usr/src/aic8800-$pkgver" "${pkgdir}/usr/src/aic8800-$pkgver"

  # Prune unwanted files
  rm -fr "${pkgdir}/usr/src/driverctl"
}
