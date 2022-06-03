# Maintainer: Ramon Santos Souza <ramonssouza93 at gmail dot com>

pkgname=kindle-nativefier
pkgver=1.0.1
pkgrel=1
pkgdesc="Kindle desktop built with nativefier (electron)"
arch=("armv7l" "i686" "x86_64")
url="https://read.amazon.com"

license=("custom")
depends=("gtk3" "libxss" "nss")
optdepends=("libindicator-gtk3")
makedepends=("imagemagick" "nodejs-nativefier" "unzip")
source=(
  "${pkgname}.png"
  "${pkgname}.desktop"
  "${pkgname}-inject.js"
)

sha256sums=('b318700996921f09103eb786529940ca3dd67dce4762e3203456d33331bbdff3'
            'c477178ed29373ce41a409a83b13d6e6d535b630997b08282ea101e0cb37cfef'
            '25d0587e3c3c9d5262778ece1e24696e51ae7617d3a458bda3d638e1683c9164')

build() {
  cd "${srcdir}"
  
  nativefier \
    --name "Kindle" \
    --icon "${pkgname}.png" \
    --width "800px" \
    --height "600px" \
    --user-agent "safari" \
    --inject "${pkgname}-inject.js" \
    --browserwindow-options '{ "webPreferences": { "spellcheck": true } }' \
    --verbose \
    --single-instance \
    --tray \
    "${url}"
}

package() {

  install -dm755 "${pkgdir}/"{opt,usr/{bin,share/{applications,licenses/${pkgname}}}}

  _folder=$(ls "${srcdir}" | grep "[Kk]indle-linux-")
  _binary=$(ls "${srcdir}/${_folder}" | grep "[Kk]indle")
  
  cp -rL "${srcdir}/${_folder}" "${pkgdir}/opt/${pkgname}"
  chmod +x "${pkgdir}/opt/${pkgname}"
  ln -s "/opt/${pkgname}/${_binary}" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm644 "${srcdir}/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
  install -Dm644 "${pkgdir}/opt/${pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  for _size in "192x192" "128x128" "96x96" "64x64" "48x48" "32x32" "24x24" "22x22" "20x20" "16x16" "8x8"
  do
    install -dm755 "${pkgdir}/usr/share/icons/hicolor/${_size}/apps"
    convert "${srcdir}/${pkgname}.png" -strip -resize "${_size}" "${pkgdir}/usr/share/icons/hicolor/${_size}/apps/${pkgname}.png"
  done
}
