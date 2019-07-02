Arch Linux ARM packages for Rock64 board. 
These packages supports hardware accelerated video playback with mpv player.  
I was able to play h264/h265 720p 30fps, and h264 1080p 24fps smoothly on 4k display.
4k playback is not watchable.

Get the built packages:

https://github.com/her01n/linux-rock64/releases/latest

And install:

    # pacman -Syu
    # pacman -U linux-rock64-4.4.167.1217-1-aarch64.pkg.tar.xz
    # pacman -U mpp-20171218-1-aarch64.pkg.tar.xz
    # pacman -U ffmpeg-mpp-1:4.1.3-1-aarch.pkg.tar.xz
    # pacamn -U libmali-rock64-1.6.3.5-1-aarch64.pkg.tar.xz
    # pacman -S mpv
    # gpasswd -a username video
    # systemctl enable gpu-governor
    # reboot
    $ mpv --hwdec=rkmpp --vo=gpu --gpu-context=drm test/h264.1080p.24f.mkv

Service "gpu-governor" would enable performance GPU governor. 
With the default 'simple_ondemand' governor the playback frame rate is very poor.

## linux-rock64
Contains kernel, modules and headers for rock64, made by ayufan. This kernel is necessary for 
hardware video acceleration. It is no longer needed for usb3 and unaccelerated hdmi output,
the stock arch linux kernel now support this.

### BUILD
```bash
$ pushd gcc7
$ makepkg --syncdeps --install
$ popd
$ pushd linux-rock64
$ makepkg --syncdeps --install
```

## gcc7
gcc7 is used to build the kernel, you do not need to install this if you only need to use the kernel.

## mpp
Media Processing Platform is a library for Rockchip VPU. This is used by all packages using 
hardware acceleration on this board.
This package would install rules to setup correct permissions on '/dev/rkvdec' on boot.
To setup it manually without reboot:

    # chgrp video /dev/rkvdec
    # chmod g+rw /dev/rkvdec

In case of this error:

    mpp_device: mpp_device_init failed to open device /dev/rkvdec, errno 13, error msg: Permission denied
You need to add your user to video group and/or reboot the computer, 
or change permissions on '/dev/rkvdec' manually.

## gstreamer-rockchip
Gstreamer plugins for hardware accelerated video decoding and encoding. 
This package is considerably lower quality than the ffmpeg-mpp and it is not recommended to use.

Example:

    $ gst-launch-1.0 filesrc location=h264.mkv ! matroskademux ! h264parse ! mppvideodec ! kmssink
    $ gst-launch-1.0 filesrc location=h265.mkv ! matroskademux ! h265parse ! mppvideodec ! kmssink
With h265 there would be a black screens and black lines displayed. I don't know how to fix that.

In case of this error:

    ERROR: from element /GstPipeline:pipeline0/GstKMSSink:kmssink0: Could not open DRM module (NULL)
You do not have set up the permissions properly, add your user to group 'video', and re-login.

## libmali-rock64
EGL+GBM drivers for rock64 graphics card. These are necessary for hardware accelerated mpv.
This package would install rules to setup correct permissions on '/dev/mali' on boot.
If you do not want to reboot, you can do it manually:

    # chgrp video /dev/mali
    # chmod g+rw /dev/mali

## ffmpeg-mpp
FFmpeg with support for mpp hardware video decoding. 
Only supports decoding for gpu output, so it is not usable on its own, only for ffmpeg based players.

## mpv
After ffmpeg-mpg, libmali-rock64, linux-rock64 and mpp are installed, 
you can use stock mpv to play video with hardware acceleration.

    $ mpv --hwdec=rkmpp --vo=gpu --gpu-context=drm h264.mkv

In case of this error:

    [vo/gpu/opengl] Failed to get EGL display.
Probably the permissions for '/dev/mali' are not set up properly, 
add your user to group video and/or reboot.

