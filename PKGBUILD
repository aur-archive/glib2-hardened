#

pkgname=glib2-hardened
pkgver=2.28.8
pkgrel=1
pkgdesc="Common C routines used by GTK+ and other libs"
url="http://www.gtk.org/"
arch=(i686 x86_64)
license=('LGPL')
depends=('pcre>=8.12')
makedepends=('pkgconfig' 'python2')
checkdepends=('pygobject' 'dbus-python')
options=('!libtool' '!docs')
replaces=('glib2<=2.28.8-1')
conflicts=('glib2>2.28.8-1')
provides=('glib2=2.28.8-1')
source=(http://ftp.gnome.org/pub/GNOME/sources/glib/2.28/glib-${pkgver}.tar.xz
        glib2.sh
        glib2.csh)
sha256sums=('4d7ca95dbde8e8f60ab428c765b0dbb8a44be9eb9316491803ce5ee7b4748353'
            '9456872cdedcc639fb679448d74b85b0facf81033e27157d2861b991823b5a2a'
            '8d5626ffa361304ad3696493c0ef041d0ab10c857f6ef32116b3e2878ecf89e3')

build() {
  pkgname="glib2"

  export CFLAGS="${CFLAGS} -fstack-protector --param ssp-buffer-size=4 -D_FORTIFY_SOURCE=2"
  export CXXFLAGS="${CFLAGS}"
  export LDFLAGS="${LDFLAGS} -Wl,-z,relro -Wl,-z,now"

  cd "${srcdir}/glib-${pkgver}"
  ./configure --prefix=/usr \
      --sysconfdir=/etc \
      --with-pcre=system \
      --disable-fam
  make
}

check() {
  pkgname="glib2"
  cd "${srcdir}/glib-${pkgver}"
  sed -i 's|!/usr/bin/env python|!/usr/bin/env python2|' gio/tests/gdbus-testserver.py
  make -k check || true
}

package() {
  pkgname="glib2"
  cd "${srcdir}/glib-${pkgver}"
  make DESTDIR="${pkgdir}" install

  install -d "${pkgdir}/etc/profile.d"
  install -m755 "${srcdir}/glib2.sh" "${pkgdir}/etc/profile.d/"
  install -m755 "${srcdir}/glib2.csh" "${pkgdir}/etc/profile.d/"

  for _i in "${pkgdir}/etc/bash_completion.d/"*; do
      chmod -x "${_i}"
  done
pkgname="glib2-hardened"
}
