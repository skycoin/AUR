# AUR
Skycoin in the Arch User Repos

Now with git subtree!

thanks to @yuvadm for the pkg.sh script!!


The old method of managing AUR packages; with $pkgname being a variable containing the name of any package
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
