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

   **NOTE**: Simply dragging in all .a files above will link your application against both OpenSSL and gnutls, which will break things. Make sure to remove all libgnutls*.a files from your XCode project after dragging them in (or just don't drag them in at all) to avoid problems!

   **NOTE**: Make sure to recreate all symbolic links after rebuilding OpenWebRTC! If you forget to do this, you may run into many obscure run-time issues.

5. If you're working on a feature in OpenWebRTC, when you're done, push it into your GitHub fork and submit a pull request.

6. If you're working on dependencies (e.g. GStreamer), use `git format-patch HEAD~1` to create a patch ready for "email submission". Then you can:
   - submit the patch to GNOME Bugzilla etc.
   - temporarily add the patch to the package's recipe (see `recipes/` folder)

7. If you're changing OpenWebRTC's Cerbero recipes, fork https://github.com/EricssonResearch/cerbero and submit a pull request against it.