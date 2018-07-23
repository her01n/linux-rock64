# linux-rock64
arch linux arm recipes for rock64 board. 
Contains kernel, modules and headers for rock64, made by ayufan.
This kernel supports usb3 port, hdmi output and probably other features, that are missing in generic arm64 kernel.
gcc7 is used to build the kernel, you do not need to install this if you only need to use the kernel.

Get the package:

https://github.com/her01n/linux-rock64/releases/download/0.6.59/linux-rock64-0.6.59-1-aarch64.pkg.tar.xz

And if you need headers for module compilation:

https://github.com/her01n/linux-rock64/releases/download/0.6.59/linux-rock64-headers-0.6.59-1-aarch64.pkg.tar.xz

BUILD

```bash
$ pushd gcc7
$ makepkg --syncdeps --install
$ popd
$ pushd linux-rock64
$ makepkg --syncdeps --install
```
