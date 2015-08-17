# Maintainer: tortorella
# Fork of `rapidminer` mantained by `mmm`
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: Bram Schoenmakers <me@bramschoenmakers.nl>

pkgname=rapidminer5
pkgver=5.3.008
pkgrel=3
pkgdesc='An open-source data mining solution.'
arch=('any')
url='http://rapid-i.com/'
license=('custom')
depends=('java-runtime')
optdepends=('gnuplot')
conflicts=('rapidminer')
source=("http://downloads.sourceforge.net/project/rapidminer/1.%20RapidMiner/${pkgver:0:3}/rapidminer-${pkgver}.zip"
        'rapidminer.desktop')
md5sums=('433d985e4c3eb6290670cf3c42d396ec'
         'e99e08980d56a5aba5ad44323a8a71cb')

package() {
  cd rapidminer

  # Determine JAVA_HOME
  [ -f /etc/profile.d/openjdk6.sh ] && source /etc/profile.d/openjdk6.sh
  [ -f /etc/profile.d/jdk.sh ] && source /etc/profile.d/jdk.sh
  [ -f /etc/profile.d/jre.sh ] && source /etc/profile.d/jre.sh
  [ -z "$JAVA_HOME" ] && ( echo "Could not determine JAVA_HOME."; return 1 )

  # Modify scripts such that the /opt/rapidminer prefix is recognized.
  # Also set the JAVA_HOME
  sed -i'' -e "s|^#RAPIDMINER_HOME=\\\${HOME}\$|RAPIDMINER_HOME=/opt/rapidminer\nJAVA_HOME=${JAVA_HOME}|" scripts/rapidminer
  sed -i'' -e "s|^#RAPIDMINER_HOME=\\\${HOME}\$|RAPIDMINER_HOME=/opt/rapidminer\nJAVA_HOME=${JAVA_HOME}|" scripts/RapidMinerGUI

  # Install files into prefix
  find {lib,resources} -type f -exec install -m 644 -D {} ${pkgdir}/opt/rapidminer/{} \;
  install -D scripts/rapidminer ${pkgdir}/opt/rapidminer/bin/rapidminer
  install -D scripts/RapidMinerGUI ${pkgdir}/opt/rapidminer/bin/RapidMinerGUI

  # custom licenses
  pushd licenses
  find . -type f -exec install -m 644 -D {} ${pkgdir}/usr/share/licenses/rapidminer/{} \;
  popd

  # install links to bins
  install -d ${pkgdir}/usr/bin
  ln -s /opt/rapidminer/bin/RapidMinerGUI \
    ${pkgdir}/usr/bin/RapidMinerGUI
  ln -s /opt/rapidminer/bin/rapidminer \
    ${pkgdir}/usr/bin/rapidminer

  # desktop file
  install -D -m644 ${srcdir}/rapidminer.desktop \
    ${pkgdir}/usr/share/applications/rapidminer.desktop
}
