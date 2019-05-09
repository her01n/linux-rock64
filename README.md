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

In case of this error:
```
mpp_device: mpp_device_init failed to open device /dev/rkvdec, errno 13, error msg: Permission denied
```
You need to add your user to video group and/or reboot the computer, 
or change permissions on '/dev/rkvdec' manually.

# gstreamer-rockchip
Gstreamer plugins for hardware accelerated video decoding and encoding.

Example:
```bash
gst-launch-1.0 filesrc location=h265.mkv ! matroskademux ! h265parse ! mppvideodec ! kmssink
gst-launch-1.0 filesrc location=h265.mkv ! matroskademux ! h265parse ! mppvideodec ! kmssink
```
With h265 there would be a black screens and black lines displayed. I don't know how to fix that.

In case of this error:
```
ERROR: from element /GstPipeline:pipeline0/GstKMSSink:kmssink0: Could not open DRM module (NULL)
```
You do not have set up the permissions properly, add your user to group 'video', and re-login.
```bash
sudo gpasswd -a username video
exit
```

# gcc7
gcc7 is used to build the kernel, you do not need to install this if you only need to use the kernel.


