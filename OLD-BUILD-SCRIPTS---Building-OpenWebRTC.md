# Platform-specific dependencies

## Host (Build) Platforms

### Mac OS X
We support the current version of **Mac OS X 10.9 Mavericks** however, it has also been tested and works fine on 10.8 Mountain Lion and 10.10 Yosemite previews.

To build on Mac OS X you need to install Xcode and the command line tools. We have used the framework with a few different versions of OS X but we try to support whatever the current stable releases are from Apple.

git must be updated to the latest version (or you have problems building dependencies). See how to update git : http://coolestguidesontheplanet.com/install-update-latest-version-git-mac-osx-10-9-mavericks/

### Linux
We currently only support **Ubuntu 14.04 LTS** and only **64-bit**. You're welcome to try building on other distributions but we probably won't be able to help with build issues at the moment.

### Windows and Other Linux
If one does not have a **64-bit Ubuntu 14.04** build environment, this may be accomplished using a liveCD. As an alternative to attempting the build on an unsupported platform; this will ensure that the build environment is properly supported, should there be any problems.
See [[Building OpenWebRTC using a LiveCD|Building-OpenWebRTC-using-a-LiveCD]]

## Target Platforms

### iOS
We currently target the **iOS 8.0** SDK.

### Android
The NDK version **r10** needs to be installed and the ndk-build program must be in the path. The latest SDK shall also be installed and the adb program needs to be available in your shell environment's PATH.

# 1. Obtaining the code

**Fork the OpenWebRTC git** and then clone it.
```
git clone --recursive https://github.com/<YourName>/openwebrtc.git
cd openwebrtc
```

# 2. Bootstrapping the build environment

Before we can start building OpenWebRTC we need to build some tools. This takes about 20-30 minutes on a dual core i5 2.4GHz running OS X 10.9.

## Available targets

Host Platform | Target Name
--- | :---:
Mac OS X 10.9 | osx
Ubuntu Linux 14.04 LTS 64-bit | linux

## Bootstrapping
This is done in the bootstrap directory:
```
cd scripts/bootstrap
./bootstrap.sh -r osx
cd -
```
If you are using a Linux computer instead of a Mac exchange the `osx` argument above to `linux`. Note: we currently only run and test on **Ubuntu 14.04 LTS** but it may work on other distributions.

On Mac if you run into the error "error: XML::Parser perl module is required for intltool" you can fix by running:

```
sudo port install -f p5-xml-parser
```

# 3. Build the dependencies
The dependencies used by OpenWebRTC framework are built separately. The amount of time may vary across targets. It takes about 50-60 minutes on a dual core i5 2.4GHz running OS X 10.9 when building just for the `osx` target. If building for multiple platforms as in the example below, it will take much longer. The `android` and `linux` targets build some different packages to the others that take quite a bit longer to build.

## Available targets

Target Platform | Target Name
--- | :---:
Mac OS X 10.9 | osx
iOS 8.0 | ios
iOS 8.0 Simulator | ios-simulator
Ubuntu Linux 14.04 LTS 64-bit | linux
Android | android

## Build the dependencies

**NOTE**: You **must** also build dependencies for your host system as well as your target system(s).

Use the following commands:
```
cd scripts/dependencies
./build-all.sh -Cbr osx
./deploy_deps.sh
cd -
```

**[Optional]** If you want to build for multiple targets simultaneously, it will take much longer but you can list them one after the other:
```
cd scripts/dependencies
./build-all.sh -Cbr osx ios ios-simulator android
./deploy_deps.sh
cd -
```

# 4. Build OpenWebRTC

## Available targets

Target Platform | Target Name
--- | :---:
Mac OS X 10.9 | osx
iOS 8.0 | ios
iOS 8.0 Simulator | ios-simulator
Ubuntu Linux 14.04 LTS 64-bit | linux
Android | android

The OpenWebRTC framework is built using one command from the root of the openwebrtc repository:
```
./build.sh -Cbr osx
```

**NOTE**: You **must** build for your host system first and then for your target system(s).

**[Optional]** Again, you can build for multiple targets simultaneously by listing them one after the other:
```
./build.sh -Cbr osx ios ios-simulator android
```

Now the OpenWebRTC framework is compiled and ready to be incorporated into your application.

# Update OpenWebRTC

In your `openwebrtc` clone:
```
git checkout master
git pull --rebase https://github.com/EricssonResearch/openwebrtc.git master
git submodule update
```

Then you may need to update dependencies...
```
cd scripts/dependencies
rm -rf openwebrtc-deps-* ../../openwebrtc-deps-*
./build-all.sh -Cbr <platforms>   # replace <platforms> as required
./deploy_deps.sh
cd -
```

And then rebuild openwebrtc...
```
./build.sh -Cbr <platforms>   # replace <platforms> as required
```

# Troubleshooting

## Bootstrap or dependencies fail to build

If part of the bootstrap or dependencies fails to build, it is most likely because your build environment has additional software that we do not have. Please check your environment variables and check you are using the supported versions of platforms and tools.

## Do I have to rebuild everything if a dependency fails to build and I fix it?

### The simple answer

If you aren't comfortable with editing shell scripts or debugging build issues, it is better if you file a report with some information about your system and the build failure.

### The very advanced answer

If you are comfortable with debugging and shell scripts and you figure out how to fix the build issue in a clean way, please submit a pull request. :smiley:

#### Fixing a failing build dependency
**The below is only for developers who know about build systems. If you start playing around with this stuff, we hope it's because you know what you're doing and are going to contribute your fixes.**

You can hop into `dependencies/build/<platform>/<package>/` to find the out of tree build directory. From there you can run `make` and so on. Running `make install` will install into `dependencies/out/<platform>/`.

If you fix it by editing the shell script and want to re-run the build completely cleanly, wiping out builds for all platforms, you can go into `dependencies/` and run `./build-<package>.sh -C` which will **completely clean out all the build and checked out code for that package**. You have been warned. :neutral_face: Then you can run `./build-<package>.sh -r <short platforms>` to try the build again.

If you fix it by editing the build system of the package (i.e. the checked out code in `dependencies/package/`) then you need to be more careful. If doing that, you can go into `dependencies/build/<platform>/<package>/` and run `make` and so on in there as stated above.

#### Continuing the dependency build process

So you fixed your issue and you want to continue building the dependencies from where you left off, instead of rebuilding everything.

You can go into `dependencies/`, make a copy of `build-all.sh` to say `continue.sh` and then remove the build scripts for the packages that built successfully. Then you can run say `./continue.sh -r <short platforms>` to continue building the rest of the dependencies.