# Building with Cerbero

## What is cerbero?

A build system written for GStreamer. We have made a few changes to existing recipes and added some more that we need for OpenWebRTC.

## Do it!

### Clone cerbero code and recipes

    cd
    git clone https://github.com/nirbheek/cerbero.git

### Platform specific bits

#### Mac OS X host

    sudo mkdir /Library/Frameworks/OpenWebRTC.framework
    sudo chown $youruser /Library/Frameworks/OpenWebRTC.framework/

In `~/cerbero`:

    mkdir -p dist
    ln -s /Library/Frameworks/OpenWebRTC.framework/Versions/0.1 dist/darwin_x86_64

### Bootstrap

    cd cerbero
    ./cerbero-uninstalled bootstrap

If building on OS X and you wish to build for iOS, also run:

    ./cerbero-uninstalled -c config/cross-ios-universal.cbc bootstrap

### Run a full build

#### Linux host build
    ./cerbero-uninstalled package openwebrtc

#### Mac OS X host build
    ./cerbero-uninstalled -c config/osx-x86-64.cbc package openwebrtc

#### iOS universal (armv7, armv7s, arm64, x86) build
    ./cerbero-uninstalled -c config/cross-ios-universal.cbc package openwebrtc

#### Android build
    ./cerbero-uninstalled -c config/cross-android-armv7.cbc package openwebrtc