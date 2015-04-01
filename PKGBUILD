# Maintainer: Isaac Aronson <i at linux dotcom>
# Contributor: Dan Serban
# Contributor: Jim Casteel
# Contributor: dryes <joswiseman@gmail>
# Contributor: Vishnevsky Roman <aka dot x0x01 at gmail dot com>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Aldo Culquicondor <alculquicondor@gmail.com>
# Contributor: Florijan Hamzic <florijanh@gmail.com>
pkgname='sphinx-beta'
pkgver=2.3.1
pkgrel=1
pkgdesc='Free open-source SQL full-text search engine.'
arch=('i686' 'x86_64')
url='http://www.sphinxsearch.com/'
license=('GPL')
conflicts=('sphinx')
depends=('unixodbc' 'expat' 'libmysqlclient' 'postgresql-libs')
optdepends=('postgresql')
backup=('etc/conf.d/sphinx')
install='sphinx.install'
source=("http://sphinxsearch.com/files/sphinx-${pkgver}-beta.tar.gz"
    'sphinx.conf.d'
    'sphinx.rc.d'
    'sphinx.service'
    'sphinx.tmpfiles.conf')

build() {
  sed -i '15199,15199 s/x00/x21/' "${srcdir}/sphinx-${pkgver}-beta/src/searchd.cpp"

  cd "${srcdir}/sphinx-${pkgver}-beta"
  ./configure --prefix=/usr --exec-prefix=/usr --localstatedir=/var/lib/sphinx \
    --sysconfdir=/etc/sphinx --with-pgsql --enable-id64

  make
}

package() {
  cd "${srcdir}/sphinx-${pkgver}-beta"

  make DESTDIR="${pkgdir}" install

  for _f in "${pkgdir}/usr/bin/"*; do
    ln -s "/usr/bin/${_f##*/}" "${pkgdir}/usr/bin/sphinx-${_f##*/}"
  done

  install -Dm755 "${srcdir}/sphinx.rc.d" "${pkgdir}/etc/rc.d/sphinx"
  install -Dm644 "${srcdir}/sphinx.conf.d" "${pkgdir}/etc/conf.d/sphinx"
  install -Dm644 "${srcdir}/sphinx.service" "${pkgdir}/usr/lib/systemd/system/sphinx.service"
  install -d "${pkgdir}/usr/share/sphinx/lib"
  install -Dm644 api/sphinxapi.php "${pkgdir}/usr/share/sphinx/lib/sphinxapi.php"
  install -Dm644 api/sphinxapi.py "${pkgdir}/usr/share/sphinx/lib/sphinxapi.py"
  install -Dm644 "${srcdir}/sphinx.tmpfiles.conf" "${pkgdir}/usr/lib/tmpfiles.d/sphinx.conf"
}
md5sums=('60647f3957dcd83b548fae3a1f5a8c98'
         '48e3e1857919d26d5104a48caffb531b'
         'faaa8310af97ff1dbdaf08612e442020'
         '335138a5893192dcf4355d3d02beb4c1'
         '22ec4cd0471a1d52702d57d78614b8d8')
