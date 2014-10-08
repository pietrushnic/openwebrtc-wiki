### HowTo build OpenWebRTC using a LiveCD

the OpenWebRTC project currently only strictly supports **64-bit Ubuntu 14.04** - however, it is not necessary to have a full install of ubuntu on your machine in order to build OpenWebRTC - this may be accomplished using a liveCD

why would one want to do this?
  * if one does not already have a linux or OSX build environment (e.g. windows)
  * if one has a linux build environment but it is not ubuntu
  * if one would like to build the entire project on a keychain to show grandma

and
  * one is targeting only linux and/or android
  * you must use a 64-bit PC

## Building OpenWebRTC using a Kubuntu LiveCD

one obvious option, would be to fully install ubuntu 14.04 to a bootable USB drive using [unetbootin](http://unetbootin.sourceforge.net/) - then you could skip this tutorial and proceed directly to [[Building OpenWebRTC|Building-OpenWebRTC]] - the procedure described here is actually simpler though and faster

if your PC has less than 2GB of RAM, you will probably need to install ubuntu onto your HD or establish a persistent liveCD nest (see [here](https://help.ubuntu.com/community/LiveCD/Persistence) or [here](https://wiki.ubuntu.com/LiveUsbPendrivePersistent))

if your PC has 2GB of RAM or more, all of the required deb packages will fit in ramdisk, so you are golden

this build is several GB however (~5.5GB), and takes a good deal of time to compile, so we will want to mount a storage device to preserve the build across boots

## Pre-install Procedure

  * download a Trusty Tahr **64-bit kubuntu 14.04 LTS Desktop LiveCD ISO*** image from [kubuntu](http://www.kubuntu.org/getkubuntu)  
    ** ASSERT: **64-bit** - - **14.04** - - **Desktop ISO** ** or better yet, use this [bittorrent link](http://cdimage.ubuntu.com/kubuntu/releases/trusty/release/kubuntu-14.04.1-desktop-amd64.iso.torrent)

  * burn to a DVD and boot or frugal boot from HD or USB with [unetbootin](http://unetbootin.sourceforge.net/) (instructions omitted for brevity)

  * choose the "Try Kubuntu" option from the boot menu to boot into the liveCD environment

  * once the kubuntu desktop becomes visible, press the ALT-F2 key combination then type `konsole` then press the ENTER key - this should open the default KDE terminal
```
    konsole
```

  * create a password for the liveCD user
```
    sudo passwd kubuntu
```

  * update the deb package cache
```
    sudo apt-get update
```

  * install and configure git
```
    sudo apt-get install git
    git config --global user.email "you@example.com"
    git config --global user.name  "Your Name"
```

  * mount a partition with ~5GB of empty space (instructions omitted for brevity)

  * cd into some dev dir and create two directories: openwebrtc and .openwebrtc
```
    cd /media/kubuntu/YOUR_PARTITION/src/
    sudo mkdir ./openwebrtc
    sudo mkdir ./.openwebrtc
```

  * make them writable for the liveCD user
```
    sudo chown kubuntu:kubuntu ./openwebrtc
    sudo chown kubuntu:kubuntu ./.openwebrtc
```

  * create a symlink in the liveCD user's home to the dir you just created
```
    ln -s /home/kubuntu/.openwebrtc /media/kubuntu/YOUR_PARTITION/src/.openwebrtc
```

  * NOTE: the standard bulid instructions will assume that you are starting in the directory that this tutorial ended in (namely: AN_EMPTY_BUILD_DIR) - the `./openwebrtc` directory that you just created is a stand-in for the initial git command to clone into - to be clear: these are probably the only two directories on this disk to which you have write access after sudo timesout - the liveCD user UID will always be the same so these directories will remain writable for the liveCD user across boots

## Build Procedure

from here follow the build instructions [[Building OpenWebRTC|Building-OpenWebRTC]]; specifying `linux` as the host build environment and `linux` and/or `android` build targets
