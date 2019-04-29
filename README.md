Arch linux packages for Rock64 board
Get the built packages:

https://github.com/her01n/linux-rock64/releases/latest

# linux-rock64
arch linux arm recipes for rock64 board. 
Contains kernel, modules and headers for rock64, made by ayufan. This kernel is necessary for 
hardware video acceleration. It is no longer needed for usb3 and unaccelerated hdmi output,
the stock arch linux kernel now support this.

## BUILD
```bash
$ pushd gcc7
$ makepkg --syncdeps --install
$ popd
$ pushd linux-rock64
$ makepkg --syncdeps --install
```

# mpp
Media Processing Platform is a library for Rockchip VPU. This is used by all packages using 
hardware acceleration on this board.

# gstreamer-rockchip
Gstreamer plugins for hardware accelerated video decoding and encoding.

Example:
```bash
gst-launch-1.0 filesrc location=test.mkv ! matroskademux ! h264parse ! mppvideodec ! kmssink
```

# gcc7
gcc7 is used to build the kernel, you do not need to install this if you only need to use the kernel.


