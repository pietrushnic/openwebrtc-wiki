# Building OpenWebRTC with Cerbero

## What is cerbero?

A build system written for GStreamer. We have made a few changes to existing recipes and added some more that we need for OpenWebRTC.

## Do it!

### Clone cerbero code and recipes
Make sure you checkout the repo in your **home directory** to avoid issues down the road.

    cd
    git clone https://github.com/nirbheek/cerbero.git

### Platform specific bits

#### Mac OS X host

    sudo mkdir /Library/Frameworks/OpenWebRTC.framework
    sudo chown $UID /Library/Frameworks/OpenWebRTC.framework/

In `~/cerbero`:

    mkdir -p dist
    mkdir -p /Library/Frameworks/OpenWebRTC.framework/Versions/0.1
    ln -s /Library/Frameworks/OpenWebRTC.framework/Versions/0.1 dist/darwin_x86_64

### Bootstrap

Bootstrapping is required for building anything with a particular config (-c argument). For example, to build using the default config:

    cd cerbero
    ./cerbero-uninstalled bootstrap

If you wish to build for iOS (requires OS X to be the host), also run:

    ./cerbero-uninstalled -c config/cross-ios-universal.cbc bootstrap

If you wish to build for Android (works with all hosts), also run:

    ./cerbero-uninstalled -c config/cross-android-armv7.cbc bootstrap

And so on. Note that as the name suggests, bootstrap only needs to be run once to prepare the build environment.

### Run a full build

**NOTE:** You **MUST** build for your host first before cross-compiling for Android/iOS. This is because some of the build results are re-used for cross-compiling.

#### Linux host build
    ./cerbero-uninstalled package openwebrtc

#### Mac OS X host build
    ./cerbero-uninstalled -c config/osx-x86-64.cbc package openwebrtc

#### iOS universal (armv7, armv7s, arm64, x86) build
    ./cerbero-uninstalled -c config/cross-ios-universal.cbc package openwebrtc

#### Android build
    ./cerbero-uninstalled -c config/cross-android-armv7.cbc package openwebrtc

## Using the results

### Mac OS X framework

On OS X, the prefix points to /Library/Frameworks/OpenWebRTC.framework/Versions/<version> but the files written there are unfortunately not directly usable. You have to move that framework out of the way and then install the appropriate .pkg files. Then if you want to update and rebuild the framework, you need to move the installed package out of the way, or remove it, and then rebuild, move the build result out of the way and install the package. It's cumbersome but we hope it will get fixed in cerbero at some point.

After building, to use the framework:
* `mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.build`
* Install the package files
** openwebrtc-0.1.0-x86_64.pkg
** openwebrtc-devel-0.1.0-x86_64.pkg
* Add the OpenWebRTC.framework to your project

Before updating your framework build:
* `sudo mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.package`
* `mv /Library/Frameworks/OpenWebRTC.framework.build /Library/Frameworks/OpenWebRTC.framework`
* Run the update build, then follow the after build steps just above

### iOS framework

### Linux Debian packages

### Android jar?

## Updating

How to check for recipe updates as well as updates in repositories for non-fixed versions and efficiently rebuild those and their dependents.

### Update cerbero and the recipes

    cd ~/cerbero
    git pull --rebase

### Fetch updated source code
    ./cerbero-uninstalled fetch

### Rebuild

Run the `./cerbero-uninstalled <optional config file> package openwebrtc` command from above.

## Development

How to edit code and rebuild it efficiently.