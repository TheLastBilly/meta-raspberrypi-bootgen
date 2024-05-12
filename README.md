# meta-raspberrypi-bootgen
This repository contains the configuration files, recipes, and dependencies needed to build a [Raspberry Pi 4](https://en.wikipedia.org/wiki/Raspberry_Pi) GNU/Linux SD Card image with the [bootgen](https://github.com/Xilinx/bootgen) command line tool using the [Yocto](https://www.yoctoproject.org/) project.

## Project Structure
- [build/conf/](build/conf): Configuration files for the image.
- [build/conf/bblayers.conf](build/conf/bblayers.conf): Layers file for the image. Lists the meta layers and recipes needed to build it.
- [build/conf/local.conf](build/conf/local.conf): Configuration file for the build. Specifies the target architecture, build options and additional packages used.
- [recipes/](recipes): Contains the recipes needed by the image.
- [recipes/bootgen_2019.2.bb](recipes/bootgen_2019.2.bb): Recipe for [bootgen](https://github.com/Xilinx/bootgen), hard-coded to version [2019.2](https://github.com/Xilinx/bootgen/tree/f9f477adf243fa40bc8c7316a7aac37a0efd426d).
- [entrypoint.sh](./entrypoint.sh): Entry point script for the Docker container. This is the first user level script ran by the container once launched.
- [poky/](./poky/): Poky package. Contains Yocto's build scripts and configuration files. Checked out at [langdale](https://wiki.yoctoproject.org/wiki/Releases) release.
- [meta-raspberrypi/](./meta-raspberrypi/): Raspberry Pi meta package. Checked out at [langdale](https://wiki.yoctoproject.org/wiki/Releases) release.
- [meta-openembedded/](./meta-openembedded/): Open embedded meta package, needed by [meta-raspberrypi](./meta-raspberrypi/). Checked out at [langdale](https://wiki.yoctoproject.org/wiki/Releases) release.
- [README.md](./README.md): This file you are reading at the moment.

## Disclaimer
This guide (and code) was written and tested on Debian 12 (Bookworm), so it is recommended that you use the same or a similar system to follow this guide. However, it should be reproducible on other GNU/Linux distributions.

## Prerequisites
- `git`
- Yocto Host Build dependencies

### Getting Prerequisites on Debian
You can install `gcc` on a Debian based system with the following command on your terminal.
```bash
sudo apt update
sudo apt install -y git
```

As for the Yocto Host Build dependencies, you should follow the [Compile Linux Distribution](https://docs.yoctoproject.org/brief-yoctoprojectqs/index.html#compatible-linux-distribution) guide on the Yoco Project's official documentation. As of the writing of this guide however, the dependencies can be installed with the following commands:
```bash
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 python3-subunit zstd liblz4-tool file locales libacl1
sudo locale-gen en_US.UTF-8

# While not listed on the official documentation, you might still needed
# to run the following command to avoid having locale issues with your
# system.
#
# After running this command you'll be taken to a text based user interface
# and will be asked which locales you'd wish to generate. Scroll down the
# list using the up/down arrow keys and select "en_US.UTF-8 UTF-8" if it's
# not selected already, then press enter. If asked which locale you'd like to
# use as the default in your system, simply select the one your system already
# had (you can find out which one that is by opening another terminal window
# and typing echo $LANG) with the up/down arrow keys and hit enter.
sudo dpkg-reconfigure locales
```

## Cloning
Before running the code on this repository, you'll have to clone it into your system. You can do so by running the following command on your terminal.
```bash
git clone https://github.com/TheLastBilly/meta-raspberrypi-bootgen
```

This will create a directory called `meta-raspberrypi-bootgen` on your system, this will be the root of this repository. Simply `cd` into it and follow the next instructions.
```bash
cd meta-raspberrypi-bootgen
```

Lastly, you'll need to initialize the repository's submodules in order to properly fetch all of their files. You can do so with the following commands:
```bash
git submodule update --init --recursive
```

## Compiling
**CAUTION: Yocto takes a significant amount of storage space (~49GB at the time of testing) to compile this image. As such, make sure you have at least 80GB of space available in your system before compiling the project, otherwise you might not be able to complete the process. And, while this is not a requirement, it is recommended you build this project from a SSD drive, as doing so from a hard drive might take significantly longer.**

The build process is largely automated by Yocto. As such, you'll just need to run the following commands to build the image.
```bash
source poky/oe-init-build-env
bitbake core-image-minimal
```

This process can take a long time to complete (~45 minutes on a Ryzen 5 2400G and 16GB of RAM from an SSD) and might make your system sluggish as a lot of processes will be created to parallelize the compilation process as much as possible.

## Deploying
TODO, pending final build
