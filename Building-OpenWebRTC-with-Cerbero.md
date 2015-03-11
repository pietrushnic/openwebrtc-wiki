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

#### Linux host build
    ./cerbero-uninstalled package openwebrtc

#### Mac OS X host build
    ./cerbero-uninstalled -c config/osx-x86-64.cbc package openwebrtc

#### iOS universal (armv7, armv7s, arm64, x86) build

**NOTE:** You **MUST** build for your host first before cross-compiling for Android/iOS. This is because some of the build results are re-used for cross-compiling.

    ./cerbero-uninstalled -c config/cross-ios-universal.cbc package openwebrtc

#### Android build

**NOTE:** You **MUST** build for your host first before cross-compiling for Android/iOS. This is because some of the build results are re-used for cross-compiling.

    ./cerbero-uninstalled -c config/cross-android-armv7.cbc package openwebrtc

## Using the results

### Mac OS X framework

On OS X, the prefix points to /Library/Frameworks/OpenWebRTC.framework/Versions/<version> but the files written there are unfortunately not directly usable. You have to move that framework out of the way and then install the appropriate .pkg files. Then if you want to update and rebuild the framework, you need to move the installed package out of the way, or remove it, and then rebuild, move the build result out of the way and install the package. It's cumbersome but we hope it will get fixed in cerbero at some point.

After building, to use the framework:
* `mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.build`
* Install the package files from `~/cerbero`
  * `openwebrtc-0.1.0-x86_64.pkg`
  * `openwebrtc-devel-0.1.0-x86_64.pkg`
* Add the OpenWebRTC.framework to your project

Before updating your framework build:
* `sudo mv /Library/Frameworks/OpenWebRTC.framework /Library/Frameworks/OpenWebRTC.framework.package`
* `mv /Library/Frameworks/OpenWebRTC.framework.build /Library/Frameworks/OpenWebRTC.framework`
* Run the update build, then follow the after build steps just above

### iOS framework
* Install the following file `openwebrtc-devel-0.1.0-ios-universal.pkg`
  * You need to change the installation destination and select "Install for me only"
* Add the OpenWebRTC.framework to your project (found in `~/Library/Developer/OpenWebRTC/iPhone.sdk/OpenWebRTC.framework`) by dragging it in to your project.
* Under Build Settings - Header Search Paths, add `~/Library/Developer/OpenWebRTC/iPhone.sdk/OpenWebRTC.framework/Headers` (recursive)

### Linux Debian packages

### Android jar?

## Updating

How to check for recipe updates as well as updates in repositories for non-fixed versions and efficiently rebuild those and their dependents.

### Update cerbero and the recipes

    cd ~/cerbero
    git pull --rebase

### Fetch updated source code
    ./cerbero-uninstalled fetch-package --full-reset --reset-rdeps openwebrtc

### Rebuild

Run the `./cerbero-uninstalled <optional config file> package openwebrtc` command from above.

## Development

This section should ultimately describe a complete (and sane) workflow for using Cerbero as a development environment.

For now, here are some techniques that prove handy:

2. Use the 'shell' command to reproduce the environment needed for building OpenWebRTC or its dependencies. For example:

   `./cerbero-uninstalled -c config/cross-ios-arm64.cbc shell`

   would launch a child shell process with the environment variables (such as `CFLAGS`, `OBJCFLAGS`, `LDFLAGS`) needed for iOS arm64 cross-compilation.

1. Do not use cross-ios-universal and osx-universal configurations during development, for two reasons:
   - they take much longer to build
   - the 'shell' feature isn't functional for 'universal' configurations since there's no "right" configuration to choose for the build environment -- e.g. arm64 and x86_64 have different `CFLAGS`

3. On OS X and iOS, during development iterations it might be faster to use libraries and headers directly (from `dist/`) rather through the OpenWebRTC.framework — to avoid having to package and deploy it for every change. For this, simply
     - use `make install` as you go through your develop-and-debug iterations,
     - have your Xcode project link with all the libraries directly from `dist/lib/` (by dragging them into your Xcode's project navigator),
     - add the following paths to your Header Search Paths

       ````
       $(HOME)/cerbero/dist/<arch>/include
       $(HOME)/cerbero/dist/<arch>/include/glib-2.0
       $(HOME)/cerbero/dist/<arch>/lib/glib-2.0/include
       $(HOME)/cerbero/dist/<arch>/include/gstreamer-1.0
       $(HOME)/cerbero/dist/<arch>/lib/gstreamer-1.0/include
       `````

     - add the following paths to your Library Search Paths for non-iOS:

       ````
       $(HOME)/cerbero/dist/<arch>/lib
       $(HOME)/cerbero/dist/<arch>/lib/gstreamer-1.0
       ````

       or for iOS:

       ````
       $(HOME)/cerbero/dist/ios_<arch>/lib/static
       $(HOME)/cerbero/dist/ios_<arch>/lib/gstreamer-1.0/static
       ````

   (Yes, this is tedious, and we should probably make something better.)

4. A common pitfall on iOS is its lack of support for dynamic libraries. Unfortunately, when Xcode sees both foo.a and foo.dylib in the same directory, it prefers dynamic linkage even for iOS projects. A workaround is to create a subdirectory with symlinks, i.e.:

    ````bash
    cd dist/ios_<arch>/lib/
    mkdir static
    cd static
    ln -s ../*.a .
    ````
    
    After this, simply drag the .a files from `dist/<platform>/lib/static/` into the Xcode project navigator. (The GStreamer static plugins already reside in `dist/<platform>/lib/gstreamer-1.0/static`.)

5. If you're working on a feature in OpenWebRTC, when you're done, push it into your GitHub fork and submit a pull request.

6. If you're working on dependencies (e.g. GStreamer), use `git format-patch HEAD~1` to create a patch ready for "email submission". Then you can:
   - submit the patch to GNOME Bugzilla etc.
   - temporarily add the patch to the package's recipe (see `recipes/` folder)

7. If you're changing OpenWebRTC's Cerbero recipes, fork https://github.com/nirbheek/cerbero and submit a pull request against it.