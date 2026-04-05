# Maintainer: Moses Narrow <moe_narrow@use.startmail.com>
pkgname=skywire-autoupdate
_pkgname=${pkgname}
pkgdesc="Skywire auto-updater: compiles latest CI-tested commit daily - debian package"
pkgver='1.0.0'
_pkgver=${pkgver}
pkgrel=1
_pkgrel=${pkgrel}
arch=('any')
_pkgarch='all'
url="https://github.com/skycoin/skywire"
makedepends=('dpkg')
depends=()
_debdeps="skywire-bin, golang"

build() {
	#create the DEBIAN/control file
	_msg2 "Creating DEBIAN/control file for ${_pkgarch}"
	echo "Package: ${_pkgname}" > ${srcdir}/${_pkgarch}.control
	echo "Version: ${_pkgver}-${_pkgrel}" >> ${srcdir}/${_pkgarch}.control
	echo "Priority: optional" >> ${srcdir}/${_pkgarch}.control
	echo "Section: web" >> ${srcdir}/${_pkgarch}.control
	echo "Architecture: ${_pkgarch}" >> ${srcdir}/${_pkgarch}.control
	echo "Depends: ${_debdeps}" >> ${srcdir}/${_pkgarch}.control
	echo "Maintainer: Skycoin" >> ${srcdir}/${_pkgarch}.control
	echo "Description: ${pkgdesc}" >> ${srcdir}/${_pkgarch}.control
	cat ${srcdir}/${_pkgarch}.control

	#create the postinstall script
	echo '#!/bin/bash
	# Create unprivileged build user
	if ! id skywire-build &>/dev/null; then
		useradd -r -m -d /var/lib/skywire-build -s /usr/sbin/nologin skywire-build
	fi
	# Add to docker group if docker is installed
	if getent group docker &>/dev/null; then
		usermod -aG docker skywire-build 2>/dev/null || true
	fi
	systemctl daemon-reload
	systemctl enable --now skywire-update.timer
' > ${srcdir}/postinst.sh
}

package() {
  _debpkgdir="${_pkgname}-${pkgver}-${_pkgrel}-${_pkgarch}"
  _pkgdir="${pkgdir}/${_debpkgdir}"
  _systemddir="etc/systemd/system"

  mkdir -p "${_pkgdir}/usr/bin"
  mkdir -p "${_pkgdir}/${_systemddir}"

  _msg2 "Installing scripts"
  install -Dm755 "${srcdir}/../skywire-update" "${_pkgdir}/usr/bin/skywire-update"
  install -Dm755 "${srcdir}/../skywire-docker-update" "${_pkgdir}/usr/bin/skywire-docker-update"

  _msg2 "Installing systemd services and timers"
  install -Dm644 "${srcdir}/../skywire-update.service" "${_pkgdir}/${_systemddir}/skywire-update.service"
  install -Dm644 "${srcdir}/../skywire-update.timer" "${_pkgdir}/${_systemddir}/skywire-update.timer"
  install -Dm644 "${srcdir}/../skywire-docker-update.service" "${_pkgdir}/${_systemddir}/skywire-docker-update.service"
  install -Dm644 "${srcdir}/../skywire-docker-update.timer" "${_pkgdir}/${_systemddir}/skywire-docker-update.timer"

  _msg2 "Installing control file and postinst script"
  install -Dm755 ${srcdir}/${_pkgarch}.control ${_pkgdir}/DEBIAN/control
  install -Dm755 ${srcdir}/postinst.sh ${_pkgdir}/DEBIAN/postinst

  _msg2 "Creating the debian package"
  cd $pkgdir
  if command -v tree &> /dev/null ; then
    _msg2 'package tree'
    tree -a ${_debpkgdir}
  fi
  dpkg-deb --build -z9 ${_debpkgdir}
  mv *.deb ../../
  #clean up manually just in case
  rm -rf ${srcdir}
  #exit so the arch package doesn't get built
  exit
}

_msg2() {
	(( QUIET )) && return
	local mesg=$1; shift
	printf "${BLUE}  ->${ALL_OFF}${BOLD} ${mesg}${ALL_OFF}\n" "$@"
}
