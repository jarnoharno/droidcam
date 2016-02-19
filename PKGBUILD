# Maintainer: Daniel Nagy <danielnagy at gmx de>

pkgname=droidcam
pkgver=6.0
pkgrel=1
_kver="`uname -r | cut -d"." -f1,2`-ARCH"
pkgdesc='A tool for using your android device as a wireless/usb webcam'
arch=('x86_64')
url='http://www.dev47apps.com/'
license=('custom')
depends=( 'bluez-libs' 'gtk2')
makedepends=( 'linux-headers' )
options=('!strip')
optdepends=('v4l-utils: Userspace tools and conversion library for Video 4 Linux'
            'xf86-video-v4l: X.org v4l video driver' )
install="$pkgname.install"
source=("$pkgname.desktop")
source+=('http://files.dev47apps.net/600/droidcam-v4l2-x64.tar.gz')
md5sums=('199d8f3dbc6697f06350b00de99f2274'
         '8a81b4f9b61ce98a34d8a916e7be9d5e')

package() {
  # Install droidcam binary file
  cd $pkgdir
  mkdir -p "$pkgdir"/usr/bin
  install -m755 "$srcdir"/${pkgname} "$pkgdir"/usr/bin/${pkgname}
  install -m755 "$srcdir"/${pkgname}-cli "$pkgdir"/usr/bin/${pkgname}-cli

  # Install the desktop icon and ".desktop" files
  install -dm0755 "${pkgdir}/usr/share/"{applications,pixmaps}
  install -m0644 "${srcdir}/icon2.png" "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
  install -m0644 "${srcdir}/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"

  mkdir -p "$pkgdir"/usr/lib/modules-load.d/
  printf "videodev\nv4l2loopback\nv4l2loopback_dc" \
         > "$pkgdir"/usr/lib/modules-load.d/droidcam.conf

  mkdir -p "$pkgdir"/etc/modprobe.d/
  printf "options v4l2loopback_dc width=320 height=240" \
         > "$pkgdir"/etc/modprobe.d/droidcam.conf

  # Install doc
  install -dm0755 "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m0644 "${srcdir}/README" "${pkgdir}/usr/share/licenses/$pkgname/README"

  # Install modules
  cd $srcdir/v4l2loopback
  sed -i -e "s,vdev->current_norm,//vdev->current_norm,g" "$srcdir"/v4l2loopback/*.c
  make
  _extramodules="extramodules-$(uname -r | cut -f-2 -d'.')-$(uname -r|sed -e 's/.*-//g')"
  MODPATH="${pkgdir}/usr/lib/modules/${_extramodules}/"
  install -Dm644 v4l2loopback-dc.ko "$MODPATH/v4l2loopback_dc.ko"
}
