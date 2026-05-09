# Maintainer: Moses Narrow <moe_narrow@use.startmail.com>
source PKGBUILD
_pkgver=${pkgver}
_pkgrel=${pkgrel}
pkgdesc="Skywire: Decentralize the web. Skycoin.com. Debian package"
_pkgarch=$(dpkg --print-architecture)
_pkgarches=('amd64' 'arm64' 'armhf' 'armel' 'riscv64' 'i386')
arch=('any')
license=('license-free')
makedepends=('dpkg')
_debdeps=""
source=("${_source[@]}"
"${source_x86_64[@]}"
"${source_i686[@]}"
"${source_aarch64[@]}"
"${source_armv7[@]}"
"${source_arm[@]}"
"${source_riscv64[@]}"
)
noextract=(
"${source_x86_64[@]}"
"${source_i686[@]}"
"${source_aarch64[@]}"
"${source_armv7[@]}"
"${source_arm[@]}"
"${source_riscv64[@]}"
)
sha256sums=('6350b0c25f1782485a3a35d9e2ed7fc34caab5eaf07cbb784323d6a87e70d66b'
            '40c80ccce9e89ae559050b943be1f09d905476c614a72d74fac2a58c821ac058'
            '00da5a9afdf5a8c7033978d2074039ba1ff7bc7a7221fbd278eb1270bdeb8eae'
            'ec24750a99f5cda8d8a8dc94743943218e1b2088c2b2c7dc1644ee78d954fe7e'
            'a6941680b5858ca3e0c85d9bf5824455a0c95524b61e42352462f2abbb750495'
            '459c78b3a9a6751a0eb9186bf2d509b5485d4ff46f938bbd03ec344ebd0ca6a2'
            '8519d027325dcb34877bb5b0fb0c3c035d7589c0046b53935e2b949d436c4be3'
            'b2be9ad04aece39759299c2333c51e81bf543fb7a6ee8f52046d499003cadf83'
            '2e0daf72fffbf81e9aa65ba0818195f9d3d43c6eb3f4656f40a4cf2f204aba4a'
            '78e80a8272d3d3fb952e249b88a55514bb419f8f9b0dc3335a9ca1d6ae01c5c5'
            '57740e8fecb39e4e4af2714214cadff6325868cf6846d9a2de4e998d8a0463e2'
            'ea6001f9dea428a6bd877676b42a2c7d6acdd36124eab2ec9d980645a55a115c'
            '0a24b82c6ac7775b541af426912091fecb34ad5cd9e741a8c6de3ac1c0ee3218'
            '03ee60eecd19c5d5260f3ae40f535c20488f045fea2f8d72d76f2778b6470809'
            '483353f172cb12c8d726dce8e0cd284ff6bf6a69b2912274559bc199b1c7f3e3'
            '60cd97d7ff821f793de68f38aad4468fc83fcddf31449397227d16a746cc8a92'
            '2f1511abbd2b42f4bfebf2a872295de5992fe98d81163ac9ab7744d61608af5e'
            '67211fd86a09a193855a3d6aae224ade46f5fff285691ff6c5705b1be08a9c42'
            'aaa05044be112dc5b15a4e965943b46f4a2022806d432b6d69ae6c11201cb847'
            '2d6629209e20c23c3ccebcbb2429083f448e52adf43e03e82a13feba86034ab1'
            'c40b43f37bccbd7ded97f9fda6c8f0d5bb504d8aa7b9abb7fe32d46ae8a3e073'
            'ff7ca35215e7c57e7ff1213fe2d440bb00939c5f07ebec1dcca930c761246755'
            'bd3a191f5afff6339ab3d61256a2e5c5247924eb8706a65a8ea3e4682da93278'
            '520b2029133edfb927f932935f29ca269789df012f23f237d58f51b2c2ba8807')

build() {
  _msg2 'creating the DEBIAN/control files'
  for _i in ${_pkgarches[@]}; do
    _msg2 "_pkgarch=${_i}"
    local _pkgarch=${_i}
    #create control file for the debian package
    echo "Package: ${pkgname}" > ${srcdir}/${_pkgarch}.control
    echo "Version: ${pkgver}-${_pkgrel}" >> ${srcdir}/${_pkgarch}.control
    echo "Priority: optional" >> ${srcdir}/${_pkgarch}.control
    echo "Section: web" >> ${srcdir}/${_pkgarch}.control
    echo "Architecture: ${_pkgarch}" >> ${srcdir}/${_pkgarch}.control
    echo "Depends: ${_debdeps}" >> ${srcdir}/${_pkgarch}.control
    echo "Provides: ${_pkgname}" >> ${srcdir}/${_pkgarch}.control
    echo "Maintainer: ${_githuborg}" >> ${srcdir}/${_pkgarch}.control
    echo "Description: ${pkgdesc}" >> ${srcdir}/${_pkgarch}.control
  done
  # Postinst mirrors skywire.install's post_install. systemd-sysusers
  # and systemd-tmpfiles do the heavy lifting (user creation, dir
  # ownership, /opt/skywire chown via the `Z` recursive-adjust line)
  # — this PKGBUILD doesn't use debhelper, so we call them
  # explicitly. Both are idempotent.
  cat > "${srcdir}/postinst.sh" <<'POSTINST_EOF'
#!/bin/bash
set -e

# Process the sysusers.d / tmpfiles.d files we shipped, so the
# _skywire user exists and /opt/skywire is owned by them BEFORE
# autoconfig runs and tries to write into the dir. systemd-sysusers
# creates the user; systemd-tmpfiles --create applies the d/Z lines.
if command -v systemd-sysusers >/dev/null 2>&1 ; then
  systemd-sysusers /usr/lib/sysusers.d/skywire.conf 2>/dev/null || true
fi
if command -v systemd-tmpfiles >/dev/null 2>&1 ; then
  systemd-tmpfiles --create /usr/lib/tmpfiles.d/skywire.conf 2>/dev/null || true
fi

# File caps for VPN apps + low-port hypervisor binds. Survives
# User= changes; required for the user-mode unit (which can't be
# granted ambient caps).
if command -v setcap >/dev/null 2>&1 ; then
  setcap 'cap_net_admin,cap_net_bind_service+eip' /opt/skywire/bin/skywire 2>/dev/null || true
fi

skywire autoconfig
POSTINST_EOF
  cat > "${srcdir}/prerm.sh" <<'PRERM_EOF'
#!/bin/bash
systemctl disable --now skywire.service 2>/dev/null || true
[[ -d /opt/skywire ]] && rm -rf /opt/skywire
rm -f /etc/systemd/system/skywire.service.d/skywire-user.conf
rmdir --ignore-fail-on-non-empty /etc/systemd/system/skywire.service.d 2>/dev/null || true
systemctl daemon-reload 2>/dev/null || true
PRERM_EOF
  _build
}

package() {

for _i in "${_pkgarches[@]}"; do
_msg2 "_pkgarch=${_i}"
local _pkgarch="${_i}"
local _pkgarch1="${_pkgarch}"
if [[ ${_pkgarch} == "armel" || ${_pkgarch} == "armhf" ]] ; then
  local _pkgarch1="arm"
fi
if [[ ${_pkgarch} == "i386" ]] ; then
  local _pkgarch1="386"
fi

local _binaryarchive="${_pkgname}-${_tag_ver}-linux-${_pkgarch1}.tar.gz"
[[ -f "${srcdir}/${_pkgname}" ]] && rm -rf "${srcdir}/${_pkgname}" || true
#[[ -d "${srcdir}/apps" ]] && rm -rf "${srcdir}/apps" || true
[[ -d "${pkgdir}/test" ]] && rm -rf "${pkgdir}/test" || true
mkdir -p "${pkgdir}/test" && cd "${pkgdir}/test"
tar -xf "${srcdir}/${_binaryarchive}"

GOBIN="${pkgdir}/test"
_GOAPPS="${GOBIN}/apps"
cp "${srcdir}/skywire-cli" "${pkgdir}/test/"
cp "${srcdir}/skywire-visor" "${pkgdir}/test/"
cp -r "${srcdir}/apps" "${_GOAPPS}"
#set up to create a .deb package
_debpkgdir="${pkgname}-${pkgver}-${pkgrel}-${_pkgarch}"
_pkgdir="${pkgdir}/${_debpkgdir}"
[[ -d "${_pkgdir}" ]] && rm -rf "${_pkgdir}"

#declare the _pkgdir and systemd directory
_systemddir="etc/systemd/system"

_package

_msg2 'installing control file and install scripts'
install -Dm755 "${srcdir}/${_pkgarch}.control" "${_pkgdir}/DEBIAN/control"
install -Dm755 "${srcdir}/postinst.sh" "${_pkgdir}/DEBIAN/postinst"
install -Dm755 "${srcdir}/prerm.sh" "${_pkgdir}/DEBIAN/prerm"
_msg2 'creating the debian package'
#create the debian package!
cd "${pkgdir}"
dpkg-deb --build -z9 "${_debpkgdir}"
mv *.deb ../../
done
#exit so the arch package doesn't get built
exit
}
