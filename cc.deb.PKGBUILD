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
sha256sums=('40c80ccce9e89ae559050b943be1f09d905476c614a72d74fac2a58c821ac058'
            '00da5a9afdf5a8c7033978d2074039ba1ff7bc7a7221fbd278eb1270bdeb8eae'
            'ec24750a99f5cda8d8a8dc94743943218e1b2088c2b2c7dc1644ee78d954fe7e'
            'a6941680b5858ca3e0c85d9bf5824455a0c95524b61e42352462f2abbb750495'
            '459c78b3a9a6751a0eb9186bf2d509b5485d4ff46f938bbd03ec344ebd0ca6a2'
            'a8221c5add4e789d89be0022a55877975c2491bf12bfdaba6d326f000017dc6e'
            'b2be9ad04aece39759299c2333c51e81bf543fb7a6ee8f52046d499003cadf83'
            '2e0daf72fffbf81e9aa65ba0818195f9d3d43c6eb3f4656f40a4cf2f204aba4a'
            '78e80a8272d3d3fb952e249b88a55514bb419f8f9b0dc3335a9ca1d6ae01c5c5'
            '57740e8fecb39e4e4af2714214cadff6325868cf6846d9a2de4e998d8a0463e2'
            'ea6001f9dea428a6bd877676b42a2c7d6acdd36124eab2ec9d980645a55a115c'
            '0a24b82c6ac7775b541af426912091fecb34ad5cd9e741a8c6de3ac1c0ee3218'
            '03ee60eecd19c5d5260f3ae40f535c20488f045fea2f8d72d76f2778b6470809'
            'a7b8ae8fdd1c0410402cb732a2c5adc5c8dc948f5f8721efa08a77bf1b9216cb'
            'ec60e0934b2a842d8609e876b53257f38084d6f02ba05aee5d37b29e8c00eda2'
            '483353f172cb12c8d726dce8e0cd284ff6bf6a69b2912274559bc199b1c7f3e3'
            '60cd97d7ff821f793de68f38aad4468fc83fcddf31449397227d16a746cc8a92'
            '2f1511abbd2b42f4bfebf2a872295de5992fe98d81163ac9ab7744d61608af5e'
            '67211fd86a09a193855a3d6aae224ade46f5fff285691ff6c5705b1be08a9c42'
            '3d7687e955de4bb5b421fe8c0e7f76bc448f8fec896fb3c946017282e730bae4'
            '92bc0f2d438fbdb1b8ce781b8812fad3e0eecd72ec6f57efaaa691bd462fc39f'
            'faf6d1d1786c2daf8460c4a209467174bd8d7558ce2b193c77c8ecb1116e0821'
            'c3ca7fff417b7cf023770bd8c58226d2ae5e03bab29cdbe95ffcc966c704f65d'
            '7d235a3e4e5e150f709b9048fc7456fe3645cf53e8b4e1d5c12b68be54603f17'
            '09f5bc6b7cc3145e23bf1d7fe567d9f07470c971198406f334ae5cf3b0254064')

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
  # Generate the deb install scripts via the shared helper from
  # PKGBUILD. Writes ${srcdir}/postinst.sh, prerm.sh, postrm.sh —
  # source of truth lives in PKGBUILD (mirrored in
  # skywire/PKGBUILD); the other consumer is skywire/deb.PKGBUILD.
  # See PKGBUILD::_gen_deb_scripts for the script bodies and the
  # SK-preservation rationale around prerm/postrm being case-aware.
  _gen_deb_scripts
  _build
}

package() {

# Generate the canonical /etc/skywire.conf ONCE using the build
# host's native binary. The per-arch loop below extracts each
# arch's binary into ${pkgdir}/test/ and points GOBIN at it, but
# those binaries can't be executed on a cross-arch build host
# (e.g. amd64 packaging arm64 → 'Exec format error'). The generated
# config text is arch-independent, so one host-native pass is
# enough and the per-arch loop just installs the cached file via
# _package's PKGBUILD-shared logic.
local _host_arch
_host_arch="$(dpkg --print-architecture 2>/dev/null || uname -m)"
local _host_pkgarch1
case "${_host_arch}" in
  x86_64|amd64)   _host_pkgarch1=amd64 ;;
  aarch64|arm64)  _host_pkgarch1=arm64 ;;
  armv7l|armhf|armel|arm)  _host_pkgarch1=arm ;;
  i686|i386)      _host_pkgarch1=386 ;;
  riscv64)        _host_pkgarch1=riscv64 ;;
  *)              _host_pkgarch1=amd64 ;;
esac
local _host_archive="${_pkgname}-${_tag_ver}-linux-${_host_pkgarch1}.tar.gz"
local _host_tmpdir
_host_tmpdir="$(mktemp -d)"
tar -xf "${srcdir}/${_host_archive}" -C "${_host_tmpdir}"
"${_host_tmpdir}/skywire" cli config gen -q > "${srcdir}/skywire.conf.generated"
chmod 640 "${srcdir}/skywire.conf.generated"
rm -rf "${_host_tmpdir}"

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
install -Dm755 "${srcdir}/postrm.sh" "${_pkgdir}/DEBIAN/postrm"

# DEBIAN/conffiles — list every editable file under /etc/ so dpkg
# preserves operator edits across upgrades. Without this file,
# every `apt upgrade skywire-bin` clobbers /etc/skywire.conf with
# the canonical template generated by `cli config gen -q`,
# silently wiping the operator's SK, HYPERVISORPKS, SKYWIRE_USER,
# etc. With it declared, dpkg keeps the old file and drops the
# new template at /etc/skywire.conf.dpkg-new, leaving the merge
# decision to the operator (apt prompts if interactive).
#
# Mirror of the AUR Arch package's `backup=("etc/skywire.conf")`
# array — same intent (conffile-style preservation), different
# packaging-system mechanism.
_msg3 'declaring /etc/skywire.conf as a conffile'
echo '/etc/skywire.conf' > "${_pkgdir}/DEBIAN/conffiles"
chmod 644 "${_pkgdir}/DEBIAN/conffiles"
_msg2 'creating the debian package'
#create the debian package!
cd "${pkgdir}"
dpkg-deb --build -z9 "${_debpkgdir}"
mv *.deb ../../
done
#exit so the arch package doesn't get built
exit
}
