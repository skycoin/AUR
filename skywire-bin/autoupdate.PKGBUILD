# Maintainer: Moses Narrow <moe_narrow@use.startmail.com>
pkgname=skywire-autoupdate
pkgdesc="Skywire auto-updater: compiles latest CI-tested commit daily and updates docker deployments"
pkgver='1.0.0'
pkgrel='1'
arch=('any')
url="https://github.com/skycoin/skywire"
depends=('skywire-bin' 'go>=1.21')
optdepends=('docker: required for skywire-docker-update'
'docker-compose: required for skywire-docker-update')
license=('license-free')
install=skywire-autoupdate.install
_scripts=("skywire-update" "skywire-docker-update")
_services=("skywire-update.service" "skywire-docker-update.service")
_timers=("skywire-update.timer" "skywire-docker-update.timer")
source=("${_scripts[@]}" "${_services[@]}" "${_timers[@]}")
sha256sums=('555211ddfeaf08d3d8ededeb8334b282cf4839ad3ab88395e425cc3bbbc2f530'
            '56a9ef48e2b4012dcfab02ef01cee5e8b92af116c5c711a27ca3dcde702ae0ea'
            '2623526259755c4979fa7eefdbc0dee74cad7b4526735a14de608dafb4fe274d'
            '45c5cdd73839e6eae9fbda46377a5447353f315821de4707a004de1c52099b80'
            '8b37eeb9ef5b2b8b7d1057d5d6e95056f7324cb4d2ac237735b7122c97c1a7fd'
            'cc6fc91077d85643250b6c76c560e6ecf1ec9f1e677576e08998289bad2b2c94')

package() {
  _systemddir="usr/lib/systemd/system"
  mkdir -p "${pkgdir}/usr/bin"
  mkdir -p "${pkgdir}/${_systemddir}"

  for _s in "${_scripts[@]}"; do
    install -Dm755 "${srcdir}/${_s}" "${pkgdir}/usr/bin/${_s}"
  done

  for _s in "${_services[@]}"; do
    install -Dm644 "${srcdir}/${_s}" "${pkgdir}/${_systemddir}/${_s}"
  done

  for _s in "${_timers[@]}"; do
    install -Dm644 "${srcdir}/${_s}" "${pkgdir}/${_systemddir}/${_s}"
  done
}
