# Building OpenWebRTC with Cerbero

## Are you sure you want to be doing this?

There are [downloadable binary releases](https://github.com/EricssonResearch/openwebrtc/releases) available so that you don't have to!

## What is cerbero?

A build system written for GStreamer. We have made a few changes to existing recipes and added some more that we need for OpenWebRTC.

## Get the Build System and Code
Make sure you checkout the repo in your **home directory** to avoid issues down the road.

    cd
    git clone https://github.com/EricssonResearch/cerbero.git

Note: To update you can then run `git checkout master && git pull --rebase`

## Prepare the host

### Mac OS X host

    sudo mkdir -p /Library/Frameworks/OpenWebRTC.framework
    sudo chown -R $UID /Library/Frameworks/OpenWebRTC.framework/

In `~/cerbero`:

    mkdir -p dist
    mkdir -p /Library/Frameworks/OpenWebRTC.framework/Versions/0.3
    ln -sf /Library/Frameworks/OpenWebRTC.framework/Versions/0.3 dist/darwin_x86_64

### Linux host

    sudo mkdir -p /opt/openwebrtc-0.3
    sudo chown -R $UID /opt/openwebrtc-0.3


## Build

First we fetch, then we bootstrap, then we package. The process is the same when building fresh as when updating.

Bootstrapping is required for building anything with a particular config (-c argument). That is you need to run the bootstrap for each target.

#### Mac OS X host target

    cd ~/cerbero \
    && ./cerbero-uninstalled -c config/osx-x86-64.cbc fetch-package --full-reset --reset-rdeps openwebrtc \
    && ./cerbero-uninstalled -c config/osx-x86-64.cbc bootstrap \
    && ./cerbero-uninstalled -c config/osx-x86-64.cbc package -f openwebrtc

#### iOS Universal cross-compile (armv7, armv7s, arm64, x86) target

**NOTE:** You **MUST** build for your host first before cross-compiling for iOS. This is because some of the build results are re-used for cross-compiling.

    cd ~/cerbero \
    && ./cerbero-uninstalled -c config/cross-ios-universal.cbc fetch-package --full-reset --reset-rdeps openwebrtc \
    && ./cerbero-uninstalled -c config/cross-ios-universal.cbc bootstrap \
    && ./cerbero-uninstalled -c config/cross-ios-universal.cbc package -f openwebrtc

#### Android cross-compile target

**NOTE:** You **MUST** build for your host first before cross-compiling for Android. This is because some of the build results are re-used for cross-compiling.

    cd ~/cerbero \
    && ./cerbero-uninstalled -c config/cross-android-armv7.cbc fetch-package --full-reset --reset-rdeps openwebrtc \
    && ./cerbero-uninstalled -c config/cross-android-armv7.cbc bootstrap \
    && ./cerbero-uninstalled -c config/cross-android-armv7.cbc package -f openwebrtc

### Linux target

    cd ~/cerbero \
    && ./cerbero-uninstalled -c config/linux.cbc fetch-package --full-reset --reset-rdeps openwebrtc \
    && ./cerbero-uninstalled -c config/linux.cbc bootstrap \
    && ./cerbero-uninstalled -c config/linux.cbc package -f openwebrtc

## Using the results

### Mac OS X framework

On OS X, the prefix points to /Library/Frameworks/OpenWebRTC.framework/Versions/<version> but the files written there are unfortunately not directly usable. You have to move that framework out of the way and then install the appropriate .pkg files. Then if you want to update and rebuild the framework, you need to move the installed package out of the way, or remove it, and then rebuild, move the build result out of the way and install the package. It's cumbersome but we hope it will get fixed in cerbero at some point.

After building, to use the framework:
* `mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.build`
* Install the package files from `~/cerbero`
  * `openwebrtc-0.3.0-x86_64.pkg`
  * `openwebrtc-devel-0.3.0-x86_64.pkg`
* Add the OpenWebRTC.framework to your project

Before updating your framework build:
* `sudo mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.package`
* `mv /Library/Frameworks/OpenWebRTC.framework.build /Library/Frameworks/OpenWebRTC.framework`
* Run the update build, then follow the after build steps just above

### iOS framework
* Install the following file `openwebrtc-devel-0.3.0-ios-universal.pkg`
  * You need to change the installation destination and select "Install for me only"
* Add the OpenWebRTC.framework to your project (found in `~/Library/Developer/OpenWebRTC/iPhone.sdk/OpenWebRTC.framework`) by dragging it in to your project.
* Under Build Settings - Header Search Paths, add `~/Library/Developer/OpenWebRTC/iPhone.sdk/OpenWebRTC.framework/Headers` (recursive)

### Linux Debian packages

Install all the Debian packages. If you used the `-c config/linux.cbc` during building then the install prefix will be `/opt/openwebrtc-0.3`. You can create an environment which uses those libraries and binaries by exporting `PATH`, `LD_LIBRARY_PATH`, `PKG_CONFIG_PATH`, `GST_PLUGIN_PATH_1_0` and so on. Such an environment should also allow you to build against the libraries.

### Android

See [OpenWebRTC-Examples/Developing-Android-apps](https://github.com/EricssonResearch/openwebrtc-examples/wiki/Developing-Android-apps)

## Updating

How to check for recipe updates as well as updates in repositories for non-fixed versions and efficiently rebuild those and their dependents.

### Update cerbero and the recipes

    cd ~/cerbero
    git checkout master
    git pull --rebase

Then follow the standard build instructions above.

## Troubleshooting

* Cross-compiling for Android/iOS, openwebrtc package fails at configure stage stating 'Exception: You need to build openwebrtc for the host darwin before you can cross-compile it for android' or iOS
** A commit may have landed between building OpenWebRTC for the host and target systems. Try rebuilding the openwebrtc package for the host and target:
```
./cerbero-uninstalled -c config/osx-x86-64.cbc buildone openwebrtc \
&& ./cerbero-uninstalled -c config/cross-ios-universal.cbc buildone openwebrtc \
&& ./cerbero-uninstalled -c config/cross-ios-universal.cbc package openwebrtc
```