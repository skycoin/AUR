# Maintainer: Moses Narrow <moe_narrow@use.startmail.com>
pkgname=skywire-bin
_pkgname=${pkgname/-bin/}
_githuborg=skycoin
pkgdesc="Skywire: Building a new Internet. Skycoin.com"
pkgver='1.3.47'
pkgrel='1'
_rc=''
#_rc='-pr1'
_pkgver="${pkgver}${_rc}"
_tag_ver="v${_pkgver}"
_pkggopath="github.com/${_githuborg}/${_pkgname}"
arch=( 'i686' 'x86_64' 'aarch64' 'armv8' 'armv7' 'armv7l' 'armv7h' 'armv6h' 'armhf' 'armel' 'arm' ) # 'riscv64' )
url="https://${_pkggopath}"
provides=( 'skywire' )
conflicts=( 'skywire' )
optdepends=('redis: required by address-resolver transport-discovery dmsg-discovery service-discovery'
'postgresql: required by transport-discovery route-finder service-discovery'
'jq: config generation for setup-node.service')
license=('license-free')
install=skywire.install
backup=("opt/${_pkgname}/{users.db,skywire.json,local}" "etc/skywire.conf")
_script=("skywire-autoconfig")
_desktop=("skywire.desktop" "skywirevpn.desktop")
_icon=("skywirevpn.png" "skywire.png")
_service=("skywire.service" "skywire-autoconfig.service" "skywire-sn.service" "skywire-ar.service" "skywire-rf.service" "skywire-tpd.service" "skywire-dmsgd.service" "skywire-dmsg.service" "skywire-sd.service")
_userservice=("skywire-user.service")
# sysusers.d / tmpfiles.d declare the _skywire service user and the
# directories it owns. Pacman's systemd hooks run systemd-sysusers
# and systemd-tmpfiles --create when the package lands these files
# under /usr/lib/sysusers.d/ and /usr/lib/tmpfiles.d/. The .install
# postinstall stays minimal as a result — only setcap, conf
# template, and `skywire autoconfig`.
_sysusers=("skywire.sysusers")
_tmpfiles=("skywire.tmpfiles")
_etcconf=("skywire.conf")
_source=("${_script[@]}"
"${_desktop[@]}"
"${_icon[@]}"
"${_service[@]}"
"${_userservice[@]}"
"${_sysusers[@]}"
"${_tmpfiles[@]}"
"${_etcconf[@]}"
"${_key[@]}")
#"https://raw.githubusercontent.com/skycoin/skywire/develop/dmsghttp-config.json"
#"https://raw.githubusercontent.com/skycoin/skywire/develop/services-config.json")
#"all_servers.json"::"https://dmsgd.skywire.skycoin.com/dmsg-discovery/all_servers")

source=("${_source[@]}")
sha256sums=('6350b0c25f1782485a3a35d9e2ed7fc34caab5eaf07cbb784323d6a87e70d66b'
            '40c80ccce9e89ae559050b943be1f09d905476c614a72d74fac2a58c821ac058'
            '00da5a9afdf5a8c7033978d2074039ba1ff7bc7a7221fbd278eb1270bdeb8eae'
            'ec24750a99f5cda8d8a8dc94743943218e1b2088c2b2c7dc1644ee78d954fe7e'
            'a6941680b5858ca3e0c85d9bf5824455a0c95524b61e42352462f2abbb750495'
            'SKIP'
            '8519d027325dcb34877bb5b0fb0c3c035d7589c0046b53935e2b949d436c4be3'
            'b2be9ad04aece39759299c2333c51e81bf543fb7a6ee8f52046d499003cadf83'
            '2e0daf72fffbf81e9aa65ba0818195f9d3d43c6eb3f4656f40a4cf2f204aba4a'
            '78e80a8272d3d3fb952e249b88a55514bb419f8f9b0dc3335a9ca1d6ae01c5c5'
            '57740e8fecb39e4e4af2714214cadff6325868cf6846d9a2de4e998d8a0463e2'
            'ea6001f9dea428a6bd877676b42a2c7d6acdd36124eab2ec9d980645a55a115c'
            '0a24b82c6ac7775b541af426912091fecb34ad5cd9e741a8c6de3ac1c0ee3218'
            '03ee60eecd19c5d5260f3ae40f535c20488f045fea2f8d72d76f2778b6470809'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')
sha256sums_i686=('ff40bcbf674cf7e1a1ffda11b1e981867c5b10e449ea6c653c245e0a9eec14f9')
sha256sums_x86_64=('f5e34a85d69ee1840ff86e59f2f18c3a5af20c139e5bc6b3553de3eb6f15dcfa')
sha256sums_aarch64=('4383b2d0471253cf65986f7d1716b9b4b7a50beeaebbb170e49ad64f7beb67cb')
sha256sums_armv8=('4383b2d0471253cf65986f7d1716b9b4b7a50beeaebbb170e49ad64f7beb67cb')
sha256sums_armv7=('ac22701d0ebf4e9ee45f11622896deb3181dc256c2a8f54e3658ee0b784ad01b')
sha256sums_armv7l=('ac22701d0ebf4e9ee45f11622896deb3181dc256c2a8f54e3658ee0b784ad01b')
sha256sums_armv7h=('ac22701d0ebf4e9ee45f11622896deb3181dc256c2a8f54e3658ee0b784ad01b')
sha256sums_arm=('64d40ab303bb74adaea9fb0a7588768482ad1c1521c6b41fec599507ea237e22')
#https://github.com/skycoin/skywire/releases/download/v1.3.32/skywire-v1.3.32-linux-amd64.tar.gz
#https://github.com/skycoin/skywire/releases/download/v1.3.32/skywire-v1.3.32-linux-arm64.tar.gz
#https://github.com/skycoin/skywire/releases/download/v1.3.32/skywire-v1.3.32-linux-armhf.tar.gz
#https://github.com/skycoin/skywire/releases/download/v1.3.32/skywire-v1.3.32-linux-arm.tar.gz
#https://github.com/skycoin/skywire/releases/download/v1.3.32/skywire-v1.3.32-linux-riscv64.tar.gz
#https://github.com/skycoin/skywire/releases/download/v1.3.32/skywire-v1.3.32-linux-386.tar.gz
_binarchive=("${_pkgname}-${_tag_ver}-linux")
_release_url=("${url}/releases/download/${_tag_ver}/${_binarchive}")
source_x86_64=("${_release_url}-amd64.tar.gz")
source_i686=("${_release_url}-386.tar.gz")
source_aarch64=("${_release_url}-arm64.tar.gz")
source_armv8=( "${source_aarch64[@]}" )
source_arm=("${_release_url}-arm.tar.gz")
source_armv7=("${_release_url}-armhf.tar.gz")
source_armv7l=( "${source_armv7[@]}" )
source_armv7h=( "${source_armv7[@]}" )
source_riscv64=( "${_release_url}-riscv64.tar.gz" )

_binaryscript=("skywire-cli" "skywire-visor")
_appscript=("skychat" "skysocks" "skysocks-client" "vpn-client" "vpn-server")

build() {
  _build
}
_build() {
  GOBIN="${srcdir}"
  _GOAPPS="${GOBIN}/apps"
  mkdir -p ${_GOAPPS}
  _msg2 'creating launcher scripts'
  echo -e '#!/bin/bash\n/opt/skywire/bin/skywire $@' > "${_GOAPPS}/skywire"
  chmod +x ${_GOAPPS}/*
  echo -e '#!/bin/bash\n/opt/skywire/bin/skywire cli $@' > "${GOBIN}/skywire-cli"
  echo -e '#!/bin/bash\n/opt/skywire/bin/skywire visor $@' > "${GOBIN}/skywire-visor"
  chmod +x ${GOBIN}/*

}

package() {
GOBIN="${srcdir}"
_GOAPPS="${GOBIN}/apps"
#declare the _pkgdir and systemd directory
_pkgdir="${pkgdir}"
_systemddir="usr/lib/systemd/system"
_skywirebin=""
_package
}
#_package function - used in build variants
_package() {
_dir="opt/skywire"
_apps="${_dir}/apps"
_bin="${_dir}/bin"
_scriptsdir="${_dir}/scripts"
_msg2 'creating dirs'
mkdir -p "${_pkgdir}/usr/bin"
#mkdir -p "${_pkgdir}/etc/logrotate.d"
mkdir -p "${_pkgdir}/${_dir}/bin"
mkdir -p "${_pkgdir}/${_dir}/apps"
mkdir -p "${_pkgdir}/${_dir}/scripts"
mkdir -p "${_pkgdir}/${_systemddir}"
_msg2 'installing scripts and binaries'
install -Dm755 "${GOBIN}/skywire" "${_pkgdir}/${_bin}/"
ln -rTsf "${_pkgdir}/${_bin}/skywire" "${_pkgdir}/usr/bin/skywire"
install -Dm755 "${GOBIN}/skywire-cli" "${_pkgdir}/${_bin}/"
ln -rTsf "${_pkgdir}/${_bin}/skywire-cli" "${_pkgdir}/usr/bin/skywire-cli"
install -Dm755 "${GOBIN}/skywire-visor" "${_pkgdir}/${_bin}/"
ln -rTsf "${_pkgdir}/${_bin}/skywire-visor" "${_pkgdir}/usr/bin/skywire-visor"
install -Dm755 "${_GOAPPS}/skywire" "${_pkgdir}/${_apps}/skywire"
for _i in "${_script[@]}" ; do
  _msg3 ${_i}
  install -Dm755 "${srcdir}/${_skywirebin}${_i}" "${_pkgdir}/${_scriptsdir}/${_i}"
  ln -rTsf "${_pkgdir}/${_scriptsdir}/${_i}" "${_pkgdir}/usr/bin/${_i}"
done

#_msg2 'installing dmsghttp-config.json'
#install -Dm644 "${srcdir}/dmsghttp-config.json" "${_pkgdir}/${_dir}/dmsghttp-config.json" || install -Dm644 "${srcdir}/skywire/dmsghttp-config.json" "${_pkgdir}/${_dir}/dmsghttp-config.json"
#_msg2 'installing services-config.json'
#install -Dm644 "${srcdir}/services-config.json" "${_pkgdir}/${_dir}/services-config.json" || install -Dm644 "${srcdir}/skywire/services-config.json" "${_pkgdir}/${_dir}/services-config.json"

_msg2 'Installing systemd services'
for _i in "${_service[@]}" ; do
  _msg3 ${_i}
  install -Dm644 "${srcdir}/${_skywirebin}${_i}" "${_pkgdir}/${_systemddir}/${_i}"
  install -Dm644 "${srcdir}/${_skywirebin}${_i}" "${_pkgdir}/etc/skel/.config/systemd/user/${_i}"
done

# User-mode systemd unit. Renamed on install (drop the -user
# suffix) so the operator runs `systemctl --user start skywire`,
# matching the system-mode `systemctl start skywire` UX. The same
# file overrides the system-unit copy under /etc/skel so newly
# created users get the user-mode unit (multi-user.target doesn't
# exist for user-systemd; the system unit copy installed by the
# loop above wouldn't actually work for them).
_msg3 'skywire-user.service → /usr/lib/systemd/user/skywire.service'
install -Dm644 "${srcdir}/${_skywirebin}skywire-user.service" "${_pkgdir}/usr/lib/systemd/user/skywire.service"
install -Dm644 "${srcdir}/${_skywirebin}skywire-user.service" "${_pkgdir}/etc/skel/.config/systemd/user/skywire.service"

_msg2 'Installing sysusers.d / tmpfiles.d (declarative user + dirs)'
install -Dm644 "${srcdir}/${_skywirebin}skywire.sysusers" "${_pkgdir}/usr/lib/sysusers.d/skywire.conf"
install -Dm644 "${srcdir}/${_skywirebin}skywire.tmpfiles" "${_pkgdir}/usr/lib/tmpfiles.d/skywire.conf"

# Default /etc/skywire.conf — installed only on first install (the
# backup= line in the PKGBUILD preserves operator edits across
# upgrades; pacman renames it to .pacnew on conflicts and lets the
# operator merge).
_msg3 'skywire.conf → /etc/skywire.conf (default template)'
install -Dm640 "${srcdir}/${_skywirebin}skywire.conf" "${_pkgdir}/etc/skywire.conf"

_msg2 'installing desktop files and icons'
mkdir -p "${_pkgdir}/usr/share/applications/" "${_pkgdir}/usr/share/icons/hicolor/48x48/apps/"
for _i in "${_desktop[@]}" ; do
  _msg3 ${_i}
  install -Dm644 "${srcdir}/${_skywirebin}${_i}" "${_pkgdir}/usr/share/applications/${_i}"
done
for _i in "${_icon[@]}" ; do
  _msg3 ${_i}
  install -Dm644 "${srcdir}/${_skywirebin}${_i}" "${_pkgdir}/usr/share/icons/hicolor/48x48/apps/${_i}"
done
if command -v tree &> /dev/null ; then
_msg2 'package tree'
  tree -a ${_pkgdir}
fi
}

_msg2() {
(( QUIET )) && return
local mesg=$1; shift
printf "${BLUE}  ->${ALL_OFF}${BOLD} ${mesg}${ALL_OFF}\n" "$@"
}

_msg3() {
(( QUIET )) && return
local mesg=$1; shift
printf "${BLUE}  -->${ALL_OFF} ${mesg}${ALL_OFF}\n" "$@"
}
