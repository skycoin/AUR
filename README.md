# AUR
Skycoin in the Arch User Repos

(Now with git subtree!)
thanks to @yuvadm for the pkg.sh script!!
^^ Needs refinement to work more easily

## Package Maintenance

This repository allows PKGBUILDs to be maintained in the [AUR](https://aur.archlinux.org) for corresponding [Skycoin]{https://github.com/skycoin} github repos.

The old method of managing AUR packages; with $pkgname being a variable containing the name of any package:
```
git clone ssh://aur@aur.archlinux.org/$pkgname.git
updpkgsums
makepkg --printsrcinfo > .SRCINFO;
git add -f PKGBUILD .SRCINFO
git commit -m "commit message"
git push
```

with `pkg.sh` and a containing repository this becomes

```
# initial setup step
./pkg.sh import $(cat pkglist.txt)
#change some files
updpkgsums
makepkg --printsrcinfo > .SRCINFO;
git add -f PKGBUILD .SRCINFO
#add scripts, etc.
git commit -m "commit message"
git push
./pkg.sh update $pkgname
```

## Testing builds

Packages are buildable with `makepkg` on arch linux, the task of handling build dependencies is fulfilled by the system package manager; `pacman`

From within any subdirectory containing a PKGBUILD (i.e. the desired repository or package to create)
```
makepkg -sf
```

## Installing from AUR

The build and installation process is fully automated with the help of `yay`
Here is an example of the installation process for the skycoin official release binaries:
```
[user@linux ~]$ yay -S skycoin-bin
:: Checking for conflicts...
:: Checking for inner conflicts...
[Aur:1]  skycoin-bin-0.27.1-2

  1 skycoin-bin                      (Installed) (Build Files Exist)
==> Packages to cleanBuild?
==> [N]one [A]ll [Ab]ort [I]nstalled [No]tInstalled or (1 2 3, 1-3, ^4)
==> a
:: Deleting (1/1): /home/d0mo/.cache/yay/skycoin-bin
:: Downloaded PKGBUILD (1/1): skycoin-bin
  1 skycoin-bin                      (Installed) (Build Files Exist)
==> Diffs to show?
==> [N]one [A]ll [Ab]ort [I]nstalled [No]tInstalled or (1 2 3, 1-3, ^4)
==>
:: (1/1) Parsing SRCINFO: skycoin-bin
==> Making package: skycoin-bin 0.27.1-2 (Sat 19 Dec 2020 04:56:26 PM CST)
==> Retrieving sources...
  -> Found skycoin-scripts.tar.gz
  -> Downloading skycoin-0.27.1-gui-standalone-linux-x64.tar.gz...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 14.3M  100 14.3M    0     0  4510k      0  0:00:03  0:00:03 --:--:-- 4509k
==> Validating source files with sha256sums...
    skycoin-scripts.tar.gz ... Passed
==> Validating source_x86_64 files with sha256sums...
    skycoin-0.27.1-gui-standalone-linux-x64.tar.gz ... Passed
==> Making package: skycoin-bin 0.27.1-2 (Sat 19 Dec 2020 04:56:34 PM CST)
==> Checking runtime dependencies...
==> Checking buildtime dependencies...
==> Retrieving sources...
  -> Found skycoin-scripts.tar.gz
  -> Found skycoin-0.27.1-gui-standalone-linux-x64.tar.gz
==> Validating source files with sha256sums...
    skycoin-scripts.tar.gz ... Passed
==> Validating source_x86_64 files with sha256sums...
    skycoin-0.27.1-gui-standalone-linux-x64.tar.gz ... Passed
==> Removing existing $srcdir/ directory...
==> Extracting sources...
  -> Extracting skycoin-scripts.tar.gz with bsdtar
  -> Extracting skycoin-0.27.1-gui-standalone-linux-x64.tar.gz with bsdtar
==> Sources are ready.
==> Making package: skycoin-bin 0.27.1-2 (Sat 19 Dec 2020 04:56:42 PM CST)
==> Checking runtime dependencies...
==> Checking buildtime dependencies...
==> WARNING: Using existing $srcdir/ tree
==> Entering fakeroot environment...
==> Starting package()...
  -> creating dirs
  -> installing binaries
  -> installing gui sources
  -> installing scripts
  -> installing systemd services
==> Tidying install...
  -> Removing libtool files...
  -> Purging unwanted files...
  -> Removing static library files...
  -> Stripping unneeded symbols from binaries and libraries...
  -> Compressing man and info pages...
==> Checking for packaging issues...
==> Creating package "skycoin-bin"...
  -> Generating .PKGINFO file...
  -> Generating .BUILDINFO file...
  -> Generating .MTREE file...
  -> Compressing package...
==> Leaving fakeroot environment.
==> Finished making: skycoin-bin 0.27.1-2 (Sat 19 Dec 2020 04:56:55 PM CST)
==> Cleaning up...
[sudo] password for user:
loading packages...
resolving dependencies...
looking for conflicting packages...

Packages (1) skycoin-bin-0.27.1-2

Total Installed Size:  21.47 MiB
Net Upgrade Size:       0.00 MiB

:: Proceed with installation? [Y/n]
(1/1) checking keys in keyring                     [----------------------] 100%
(1/1) checking package integrity                   [----------------------] 100%
(1/1) loading package files                        [----------------------] 100%
(1/1) checking for file conflicts                  [----------------------] 100%
:: Processing package changes...
(1/1) upgrading skycoin-bin                        [----------------------] 100%
:: Running post-transaction hooks...
(1/2) Reloading system manager configuration...
(2/2) Arming ConditionNeedsUpdate...
```

The package was built and installed according to the functions defined in the PKGBUILD. For skycoin-bin, only the `package()` function is used. The source for the release is checked against its sha256sum.

## Packaging Notes

### basic limits-

* makepkg cannot be run as root
* no installing into a user's `$HOME` folder

### Standardization and etiquitte-

* don't rely on makepkg internal functions
* declare additional variables with leading _underscore

### Specific guidelines for skycoin packages-

* **The installation directory should be a subdirectory of `/opt`**
* **Do not ever use inbuilt skywire updater with a package**
* Scripts are provided to enhance user experience and provide automation

###  Naming Conventions and build specifications / types

* **Binaries are statically compiled with `musl` now by default**
* `-bin` denotes a package which uses official release binaries.
* without suffix, a package is built either from the source archive of the latest release or from the default branch if no source archive exists

Within the cloned AUR repository, a few variations of the PKGBUILD are sometimes included

* `deb.PKGBUILD` build debian package
* `cc.deb.PKGBUILD` build debian packages for all architectures
* `git.PKGBUILD` build using cloned .git sources

## Skywire packaging overview

The goal of the skywire AUR package is to have only one command between the running system without skywire and the running instance of skywire. Even without the existance of a hosted release package. The build / install is handled by `yay` and the configuration is defined in the `skywire-autoconfig` script which is called by the .install script at the point of installation.

## deb.PKGBUILDS

Debian packages can now be natively created with archlinux packaging tools.

A typical example of building a debian package on archlinux

Fiirst, install the package with `yay` to the archlinux host system
```
yay -S skywire-bin
```

switch to the directory where the package was compiled in yay's cache and run makepkg on the deb.PKGBUILD

```
cd ~/.cache/yay/skywire-bin
makepkg -p deb.PKGBUILD
```
The debian package is produced.

