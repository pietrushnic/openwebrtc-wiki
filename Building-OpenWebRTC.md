# Platform-specific dependencies

## Host (Build) Platforms

### Mac OS X
To build on Mac OS X you need to install Xcode and the command line tools. We have used the framework with a few different versions of OS X but we try to support whatever the current stable releases are from Apple.

### Linux
We currently only support **Ubuntu 14.04 LTS**. You're welcome to try building on other distributions but we probably won't be able to help with build issues at the moment.

## Target Platforms

### iOS
We currently target the iOS 8 SDK.

### Android
The NDK version r9d needs to be installed and the ndk-build program must be in the path. The latest SDK shall also be installed and the adb program needs to be available in your shell environment's PATH.

# Obtaining the code
**Fork the OpenWebRTC git** and then clone it.
```
git clone https://github.com/<YourName>/openwebrtc.git --recursive
cd openwebrtc
```
# Environment configuration
Before we can start building OpenWebRTC we need to build some tools. This is done in the bootstrap directory:
```
cd scripts/bootstrap
./bootstrap.sh -r osx
cd -
```
If you are using a Linux computer instead of a Mac exchange the "osx" argument above to "linux". Note: we currently only run and test on **Ubuntu 14.04 LTS** but it may work on other distributions.

# Build the dependencies
The dependencies used by OpenWebRTC framework are built separately using the following commands:
```
cd scripts/dependencies
./build-all.sh -r osx ios android ios-simulator
./deploy_deps.sh
cd -
```
The available target platforms when building on Mac can be seen above, if you are building on Linux the available target platforms are "linux" and "android".

# Build OpenWebRTC

The OpenWebRTC framework is built using one command:
```
./build.sh -r osx ios android ios-simulator
```
The available target platform when building on Mac can be seen above, if you are building on Linux the available target platforms are "linux" and "android".

Now the OpenWebRTC framework is compiled and ready to be incorporated into your application.

# Troubleshooting

## Bootstrap or dependencies fail to build

If part of the bootstrap or dependencies fails to build, it is most likely because your build environment has additional software that we do not have. Please check your environment variables and check you are using the supported versions of platforms and tools.

## Do I have to rebuild everything if a dependency fails to build and I fix it?

### The simple answer

Only if you know what you're doing. If you aren't comfortable with editing shell scripts or debugging build issues, it is better if you file a report with some information about your system and the build failure.

### The very advanced answer

If you are comfortable with debugging and shell scripts and you figure out how to fix the build issue in a clean way, please submit a pull request. :)

#### Fixing a failing build dependency
**The below is only for developers who know about build systems. If you start playing around with this stuff, we hope it's because you know what you're doing and are going to contribute your fixes.**

You can hop into `dependencies/build/<platform>/<package>/` to find the out of tree build directory. From there you can run `make` and so on. Running `make install` will install into `dependencies/out/<platform>/`.

If you fix it by editing the shell script and want to re-run the build completely cleanly, wiping out builds for all platforms, you can go into `dependencies/` and run `./build-<package>.sh -C` which will **completely clean out all the build and checked out code for that package**. You have been warned. :) Then you can run `./build-<package>.sh -r <short platforms>` to try the build again.

If you fix it by editing the build system of the package (i.e. the checked out code in `dependencies/package/`) then you need to be more careful. If doing that, you can go into `dependencies/build/<platform>/<package>/` and run `make` and so on in there as stated above.

#### Continuing the dependency build process

So you fixed your issue and you want to continue building the dependencies from where you left off, instead of rebuilding everything.

You can go into `dependencies/`, make a copy of `build-all.sh` to say `continue.sh` and then remove the build scripts for the packages that built successfully. Then you can run say `./continue.sh -r <short platforms>` to continue building the rest of the dependencies.