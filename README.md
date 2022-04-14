# VirtualBox OSE (Open Source Edition)

## Installation

The instruction below is for Linux distributions, which can be read from the [VirtualBox OSE Installation Manual](https://www.virtualbox.org/wiki/Linux%20build%20instructions).

### Prerequisites

First off, you have to install the required packages:

    apt-get install acpica-tools chrpath doxygen g++-multilib libasound2-dev libcap-dev \
        libcurl4-openssl-dev libdevmapper-dev libidl-dev libopus-dev libpam0g-dev \
        libpulse-dev libqt5opengl5-dev libqt5x11extras5-dev qttools5-dev libsdl1.2-dev libsdl-ttf2.0-dev \
        libssl-dev libvpx-dev libxcursor-dev libxinerama-dev libxml2-dev libxml2-utils \
        libxmu-dev libxrandr-dev make nasm python3-dev python-dev qttools5-dev-tools \
        texlive texlive-fonts-extra texlive-latex-extra unzip xsltproc \
        \
        default-jdk libstdc++5 libxslt1-dev linux-kernel-headers makeself \
        mesa-common-dev subversion yasm zlib1g-dev

Extra packages for python hacking:

    apt-get install pylint python3-psycopg2 python3-willow  python-psycopg2 python-pil

### Building VirtualBox

1. Change to the source code directory and execute the configure script:

        ./configure --disable-hardening

If it finds everything it needs, it will create a file called 'AutoConfig.kmk' containing paths to the various tools on your system. Also, it will create an environment setup script called env.sh. This step only has to be done once (if something changes in your build tool setup, you might have to repeat it but keep in mind that both output files will be overwritten).

2. Execute the `env.sh` file to setup the environment:

        source ./env.sh

3. To build a release package, type

        kmk all

    For a debug build, type

        kmk BUILD_TYPE=debug

    By default, `kmk` will automatically do a parallel build in case you have more than 1 CPU core. You can use the `-j <number of jobs>` option to override this.

### Running your build

You can run VirtualBox directly from the build target directory `(out/linux.amd64/release/bin)` if the build is not hardened. But first of all, you must build and install the VirtualBox kernel module by issuing the following:

```bash
cd out/linux.x86/release/bin/src
make
sudo make install  # Installing kernel modules require root privileges
cd ..
```

After installing the module, you can load it with `modprobe vboxdrv`. In case you don't have access to the driver, you may give yourself read and write access to `/dev/vboxdrv`:

```bash
# Add the current user to the 'vboxusers' group
groupadd vboxusers && usermod -a -G vboxusers $USER

# Add read and write access to the driver
chmod 660 /dev/vboxdrv && chgrp vboxusers /dev/vboxdrv
```

After that, you can run VirtualBox by executing the following:

    ./VirtualBox
