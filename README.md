# meta-raspberrypi-bootgen
This repository contains the configuration files, recipes, and dependencies needed to build a [Raspberry Pi 4](https://en.wikipedia.org/wiki/Raspberry_Pi) GNU/Linux SD Card image with the [bootgen](https://github.com/Xilinx/bootgen) command line tool using the [Yocto](https://www.yoctoproject.org/) project.

## Project Structure
- [./build/conf/](build/conf): Configuration files for the image.
- [./build/conf/bblayers.conf](build/conf/bblayers.conf): Layers file for the image. Lists the meta layers and recipes needed to build it.
- [./build/conf/local.conf](build/conf/local.conf): Configuration file for the build. Specifies the target architecture, build options and additional packages used.
- [./recipes/](recipes): Contains the recipes needed by the image.
- [./recipes/bootgen_2019.2.bb](recipes/bootgen_2019.2.bb): Recipe for [bootgen](https://github.com/Xilinx/bootgen), hard-coded to version [2019.2](https://github.com/Xilinx/bootgen/tree/f9f477adf243fa40bc8c7316a7aac37a0efd426d).
- [entrypoint.sh](./entrypoint.sh): Entry point script for the Docker container. This is the first user level script ran by the container once launched.
- [./meta-openembedded/](./meta-openembedded/): Open embedded meta package.
- [README.md](./README.md): This file you are reading at the moment.
