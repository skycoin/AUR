# Maintainer: Moses Narrow <moe_narrow@use.startmail.com>
source PKGBUILD
_pkgver=${pkgver}
_pkgrel=${pkgrel}
pkgdesc="Skywire: Decentralize the web. Skycoin.com. Debian package"
_pkgarch=$(dpkg --print-architecture)
_pkgarches=('amd64' 'arm64' 'armhf' 'armel')
arch=('any')
license=('license-free')
makedepends=('dpkg')
_debdeps=""
_binarchive=("${_pkgname}-${_tag_ver}-linux")
_release_url=("${url}/releases/download/${_tag_ver}/${_binarchive}")
source=("${_source[@]}"
"${_release_url}-amd64.tar.gz"
"${_release_url}-arm64.tar.gz"
"${_release_url}-armhf.tar.gz"
"${_release_url}-arm.tar.gz"
"postinst.sh"
"prerm.sh"
)
noextract=(
"${_binarchive}-amd64.tar.gz"
"${_binarchive}-arm64.tar.gz"
"${_binarchive}-armhf.tar.gz"
"${_binarchive}-arm.tar.gz"
)
sha256sums=('21775a1917f7fabea78d2b631a70c6989b910ee191b822c65fb6f290458dbced'
            '40c80ccce9e89ae559050b943be1f09d905476c614a72d74fac2a58c821ac058'
            '00da5a9afdf5a8c7033978d2074039ba1ff7bc7a7221fbd278eb1270bdeb8eae'
            'ec24750a99f5cda8d8a8dc94743943218e1b2088c2b2c7dc1644ee78d954fe7e'
            'a6941680b5858ca3e0c85d9bf5824455a0c95524b61e42352462f2abbb750495'
            '44a25adf22c87bf7a2102a7fc1c9f566d239ef3f3d7b3dc2bcd0f2c632695a17'
            '8519d027325dcb34877bb5b0fb0c3c035d7589c0046b53935e2b949d436c4be3'
            '41c0a4a42ae64479b008392053f4a947618acd6bb9c3ed2672dafdb2453caa14'
            'd6de9eaaafcbe0117e70be2bf490f1d43ce8c0ffb6348d68e24c7d4175025a53'
            'f558abaa1de6ffc7c70ffefbeb691bc44ca6c0ee64802f0a6ba6f57e2b5a2e2f'
            '500906d22415c2851b87b87bdf2c1caa066908101555e90254a9ae57c93bf43b'
            '4de6faf7eb8b97f9dc87397ab221eaa935141d2a92948d2abfca5d7fbb79274a'
            '7b17a55e64d1371fecd24edc3da7c6a762cf2b3d058f6fc4a5dc7c5f9b4cff09'
            '233ccf0e87b37e782828f53960e05e478cf381f61bbfdb16a0cc0254ee8be7c2')

build() {
  _msg2 'creating the DEBIAN/control files'
  for _i in ${_pkgarches[@]}; do
    [[ ${_i} == "armel" ]] && continue
    _msg2 "_pkgarch=${_i}"
    local _pkgarch=${_i}
    #create control file for the debian package
    echo "Package: ${pkgname}" > ${srcdir}/${_pkgarch}.control
    echo "Version: ${pkgver}-${_pkgrel}" >> ${srcdir}/${_pkgarch}.control
    echo "Priority: optional" >> ${srcdir}/${_pkgarch}.control
    echo "Section: web" >> ${srcdir}/${_pkgarch}.control
#    if [[ ${_pkgarch} == "armhf" ]] ; then
#      echo "Architecture: armhf armel" >> ${srcdir}/${_pkgarch}.control
#    else
      echo "Architecture: ${_pkgarch}" >> ${srcdir}/${_pkgarch}.control
#    fi
    echo "Depends: ${_debdeps}" >> ${srcdir}/${_pkgarch}.control
    echo "Provides: ${_pkgname}" >> ${srcdir}/${_pkgarch}.control
    echo "Maintainer: ${_githuborg}" >> ${srcdir}/${_pkgarch}.control
    echo "Description: ${pkgdesc}" >> ${srcdir}/${_pkgarch}.control
  done
}

package() {

for _i in "${_pkgarches[@]}"; do
#  [[ ${_i} == "armel" ]] && continue

_msg2 "_pkgarch=${_i}"
local _pkgarch="${_i}"
local _pkgarch1="${_pkgarch}"
if [[ ${_pkgarch} == "armel" || ${_pkgarch} == "armhf" ]] ; then
  local _pkgarch1=arm
fi

local _binaryarchive="${_pkgname}-${_tag_ver}-linux-${_pkgarch1}.tar.gz"
[[ -f "${srcdir}/${_pkgname}-visor" ]] && rm -rf "${srcdir}/${_pkgname}-visor"
[[ -f "${srcdir}/${_pkgname}-cli" ]] && rm -rf "${srcdir}/${_pkgname}-cli"
[[ -d "${srcdir}/apps" ]] && rm -rf "${srcdir}/apps"
[[ -d ${pkgdir}/test ]] && rm -rf ${pkgdir}/test
mkdir -p "${pkgdir}/test" && cd "${pkgdir}/test"
tar -xf "${srcdir}/${_binaryarchive}"

GOBIN="${pkgdir}/test"
_GOAPPS="${GOBIN}/apps"
#set up to create a .deb package
_debpkgdir="${pkgname}-${pkgver}-${pkgrel}-${_pkgarch}"
_pkgdir="${pkgdir}/${_debpkgdir}"
[[ -d "${_pkgdir}" ]] && rm -rf "${_pkgdir}"

#declare the _pkgdir and systemd directory
_systemddir="etc/systemd/system"

_package

_msg2 'installing control file and install scripts'
install -Dm755 "${srcdir}/${_pkgarch}.control" "${_pkgdir}/DEBIAN/control"
#install -Dm755 ${srcdir}/${_scripts}/preinst.sh ${_pkgdir}/DEBIAN/preinst
install -Dm755 "${srcdir}/postinst.sh" "${_pkgdir}/DEBIAN/postinst"
install -Dm755 "${srcdir}/prerm.sh" "${_pkgdir}/DEBIAN/prerm"
#install -Dm755 "${srcdir}/${_scripts}/postrm.sh" "${_pkgdir}/DEBIAN/postrm"

_msg2 'creating the debian package'
#create the debian package!
cd "${pkgdir}"
dpkg-deb --build -z9 "${_debpkgdir}"
mv *.deb ../../
done
#exit so the arch package doesn't get built
exit
}
