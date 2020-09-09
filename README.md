# Lichee Pi Nano Bootable Linux Image (Buildroot)

[Lichee Pi Nano](http://nano.lichee.pro/index.html) ([English article](https://www.cnx-software.com/2018/08/17/licheepi-nano-cheap-sd-card-sized-linux-board/)) is a very small single-board computer that is about the size of an SD card. It can run Linux.
There is a good amount of official documentation on the [original manufacturer site](http://nano.lichee.pro/get_started/first_eye.html).

This repository contains a Buildroot config extension. That means fully building the U-Boot image, Linux kernel, the rootfs image and the final partitioned binary image for flashing onto the bootable SD card.

This repo clone from: https://github.com/unframework/licheepi-nano-buildroot. This repo slightly changhed for my own purpose (for example enable USB OTG support and disabled LCD). 

There was reasonable effort to keep config files readable, e.g. here is the main Buildroot defconfig file: [configs/licheepi_nano_defconfig](configs/licheepi_nano_defconfig).

## Building the Image

download Buildroot and extract it into a folder.

Before building, install these Ubuntu packages:

```sh
sudo apt install swig python-dev fakeroot devscripts
```

If there are still error messages during later build, try installing these (sorry, did not clean up the list yet, some might be unnecessary):

```sh
sudo apt install -y chrpath gawk texinfo libsdl1.2-dev whiptail diffstat cpio libssl-dev
```

Then, create initial build configuration:

```sh
BR2_EXTERNAL=<path_to_this_repo> make licheepi_nano_defconfig
```

Customize Buildroot configuration if needed:

```sh
make menuconfig
```

Proceed with the build:

```sh
make
```

The build may take 1.5 hours on a decent machine, or longer. 

A successful build will produce a `output/images` folder. That folder contains a `sdcard.img` file that can now be written to the bootable SD card. For example:

```sh
sudo dd if=output/images/sdcard.img of=/dev/sdX bs=1M; sync
```

