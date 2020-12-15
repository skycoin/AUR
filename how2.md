```
git clone ssh://aur@aur.archlinux.org/$pkgname.git
updpkgsums
makepkg --printsrcinfo > .SRCINFO;
git add -f PKGBUILD .SRCINFO
git commit -m "commit message"
git push
```
