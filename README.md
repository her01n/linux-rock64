# linux-rock64
arch linux arm recipes for rock64 board. contains kernel, modules and headers for rock64, made by ayufan.
This kernel supports usb3 port, hdmi output and probably other features, that are missing in generic arm64 kernel.
gcc7 is used to build the kernel, you do not need to install this if you only need to use the kernel.

BUILD AND INSTALL

$ pushd gcc7
$ makepkg --syncdeps --install
$ popd
$ pushd linux-rock64
$ makepkg --syncdeps --install

