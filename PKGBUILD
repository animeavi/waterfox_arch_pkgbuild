# Maintainer: Matt/ilikenwf <parwok@gmail.com>
# based on firefox-dev
# Contributor: <jnbek1972 at gmail dot com>
# Contributor: <raku at rakutiki.tv>
pkgname=waterfox-g4-zero
pkgver=4.0.4
pkgrel=1
pkgdesc="64-Bit optimized Firefox fork, no data collection, allows unsigned extensions"
arch=('i686' 'x86_64')
license=('MPL')
url="https://www.waterfoxproject.org/"
depends=('gtk2' 'gtk3' 'libxt' 'startup-notification'
         'dbus-glib' 'alsa-lib' 'ffmpeg' 'desktop-file-utils' 'hicolor-icon-theme'
         'libvpx' 'icu' 'libevent' 'nss' 'hunspell' 'sqlite' 'ttf-font')
makedepends=('unzip' 'zip' 'diffutils' 'python2' 'yasm' 'mesa' 'imake' 'gconf'
             'xorg-server-xvfb' 'libpulse' 'inetutils' 'autoconf2.13' 'clang' 'llvm' 'cargo')
provides=("waterfox=$pkgver")
conflicts=('waterfox' 'waterfox-bin' 'waterfox-git' 'waterfox-g4-bin')
install=waterfox.install
options=('!emptydirs' '!makeflags' '!strip')
source=(git+https://github.com/animeavi/waterfox2.git
        mozconfig
        waterfox.desktop
        vendor.js)
sha512sums=('SKIP'
'SKIP'
'SKIP'
'SKIP')
            
# don't compress the package - we're just going to uncompress during install in a moment
PKGEXT='.pkg.tar'   
 
prepare() {
  cd waterfox2
  cp ../mozconfig .mozconfig
  mkdir -p "$srcdir/path"
}

build() {
  cd waterfox2
  export PATH="$srcdir/path:$PATH"
  export PYTHON="/usr/bin/python2"
  MACH_USE_SYSTEM_PYTHON=true ./mach build
}

package() {
  cd waterfox2
  mkdir -p "$pkgdir"
  DESTDIR="$pkgdir" MACH_USE_SYSTEM_PYTHON=true ./mach install
  install -Dm644 ../vendor.js "$pkgdir/opt/waterfox/browser/defaults/preferences/vendor.js"
  for i in 16 32 48; do
      install -Dm644 "$srcdir/waterfox2/obj-x86_64-pc-linux-gnu/dist/waterfox/browser/chrome/icons/default/default$i.png" \
        "$pkgdir/usr/share/icons/hicolor/${i}x${i}/apps/waterfox.png"
  done
  install -Dm644 "$srcdir/waterfox2/obj-x86_64-pc-linux-gnu/dist/waterfox/browser/chrome/icons/default/default128.png" \
    "$pkgdir/usr/share/icons/hicolor/64x64/apps/waterfox.png"
  install -Dm644 "$srcdir/waterfox2/obj-x86_64-pc-linux-gnu/dist/waterfox/browser/chrome/icons/default/default128.png" \
    "$pkgdir/usr/share/icons/hicolor/128x128/apps/waterfox.png"
  install -Dm644 browser/branding/unofficial/content/about-logo.png \
    "$pkgdir/usr/share/icons/hicolor/192x192/apps/waterfox.png"
  install -Dm644 browser/branding/unofficial/content/about-logo@2x.png \
    "$pkgdir/usr/share/icons/hicolor/384x384/apps/waterfox.png"
  install -Dm644 ../waterfox.desktop \
    "$pkgdir/usr/share/applications/waterfox.desktop"

  # Use system-provided dictionaries
  rm -rf "$pkgdir"/opt/waterfox/{dictionaries,hyphenation}
  ln -s /usr/share/hunspell "$pkgdir/opt/waterfox"
  ln -s /usr/share/hyphen "$pkgdir/opt/waterfox"
}

