There are many projects required by OpenWebRTC to provide different parts of the functionality needed for WebRTC. This page is meant to help summaries the dependencies that are used by the project as well as their software licenses. For specific licenses, you need to find each project's license text - [see here for an outdated list](http://www.openwebrtc.org/third-party-licenses).

**This page is not intended to be an introduction to how to work with Open Source software, rather a helping hand once you know that.**

**Also, the authors of this page are not lawyers. As when using any software in your own application, it is your responsibility to adhere to the licenses of that software.**

## Overview

OpenWebRTC has a mix of licenses amongst its dependencies - BSD, LGPL, MIT, etc. Each license has its own requirements, normally the copyright and license needs to be presented in any application written using the software. Some licenses have more significant requirements.

### OpenSSL

OpenWebRTC uses OpenSSL for various encryption/decryption tasks. The OpenSSL licenses are said to be incompatible with GPL. You therefore cannot use OpenSSL as part of a GPL application and must modify the various dependencies of OpenWebRTC to use other cryptography libraries, such as gnutls if you wish to write a GPL application. This does not impact applications whose own license is not GPL.

### LGPL v2 / v2.1

When packaged together, the requirements of all licenses must be met. However, BSD, MIT and similar licenses tend to only require that you present the copyright and license text in your application. LGPL v2 / v2.1 and later have stronger requirements. You must adhere to these requirements if you wish to use OpenWebRTC in an application.

It is recommended that you read the LGPL license texts:
* [LGPL v2](https://www.gnu.org/licenses/old-licenses/lgpl-2.0.html)
* [LGPL v2.1](https://www.gnu.org/licenses/lgpl-2.1.html)

To our non-lawyer eyes, an application using OpenWebRTC needs to adhere to at least one of the points in Section 6. As our attempt to help, when making releases, we also publish source code tarballs that include the cerbero build system as well as all the source code that gets downloaded when building for that target. This then contains all of the source code that is actually built by cerbero to make the OpenWebRTC binary releases.

## Build System

Project | License
:-:|:-:
cerbero | LGPLv2.1

## Build Tools
	
Project | License
:-:|:-:
android-sdk | Apache 2.0 (/GPLv2 for some code portions)
autoconf | GPLv3+
automake | GPLv2+
bison | GPLv3+
bzip2 | BSD
flex | BSD
gas-preprocessor | GPLv2
gettext | GPLv3
gnome-common | GPLv2
gperf | GPLv2
gtk-doc-lite | GPLv2+ / GPLv3
intltool | GPLv2
libtool | GPLv2
m4 | GPLv2
ndk | Apache 2.0 (/GPLv2 for some code portions)
pkg-config | GPLv2
sed | 
tar | GPLv3
xz | GPLv3
yasm | GPLv3

## Dependencies

Project | License
:-:|:-:
bionic-fixup | Apache 2.0
glib | LGPLv2+
gnustl | LGPLv2.1
gobject-introspection | GPLv2 tools / LGPLv2.1 library
gst-plugins-bad-1.0 | Mixed, depends on the plugin
gst-plugins-base-1.0 | LGPLv2.1+
gst-plugins-good-1.0 | LGPLv2.1+
gstreamer-1.0 | LGPLv2.1+
icu | ICU License: http://source.icu-project.org/repos/icu/icu/trunk/license.html
javascriptcoregtk | LGPLv2.1/BSD
json-glib | LGPLv2.1+
libffi | BSD
libiconv | GPLv3+ tools / LGPLv2+ library
libnice | LGPLv2.1+ and MPL 1.1
libsoup | LGPLv2
libsrtp | BSD
libusrsctp | BSD
libvpx | BSD
libxml2 | MIT
openh264 | BSD
openssl | BSD-style but incompatible with GPL https://github.com/openssl/openssl/blob/master/LICENSE
opus | BSD
orc | BSD
seed | LGPLv2+
zlib | BSD

## GStreamer Plugins

Project | Plugin | License
:-:|:-:|:-:
gst-plugins-bad | videoparsersbad | LGPLv2.1+
gst-plugins-good | alaw | LGPLv2.1+
gst-plugins-good | mulaw | LGPLv2.1+
gst-plugins-base | app | LGPLv2.1+
gstreamer | coreelements | LGPLv2.1+
gst-plugins-bad | inter | LGPLv2.1+
gst-plugins-base | videorate | LGPLv2.1+
openwebrtc-gst-plugins | androidvideosource | BSD 2-clause
gst-plugins-bad | applemedia | LGPLv2.1+
gst-plugins-bad | opensles | LGPLv2+
gst-plugins-good | osxaudio | LGPLv2.1+
gst-plugins-good | video4linux2 | LGPLv2.1+
gst-plugins-base | audioconvert | LGPLv2.1+
gst-plugins-base | audioresample | LGPLv2.1+
openwebrtc-gst-plugins | ercolorspace | BSD 2-clause
gst-plugins-base | videoconvert | LGPLv2.1+
gst-plugins-good | videocrop | LGPLv2.1+
gst-plugins-good | videofilter | LGPLv2.1+
gst-plugins-base | videoscale | LGPLv2.1+
gst-plugins-base | volume | LGPLv2.1+
gst-plugins-good | rtp | LGPLv2.1+
gst-plugins-good | rtpmanager | LGPLv2.1+
gst-plugins-base | audiotestsrc | LGPLv2.1+
gst-plugins-base | videotestsrc | LGPLv2.1+
gst-plugins-bad | opengl | LGPLv2.1+
gst-plugins-good | vpx | LGPLv2.1+
gst-plugins-bad (was in openwebrtc-gst-plugins) | dtls | BSD 2-clause
libnice | nice | LGPLv2.1+
gst-plugins-bad (was in openwebrtc-gst-plugins) | openh264 | BSD 2-clause
gst-plugins-good | pulseaudio | LGPLv2.1+
openwebrtc-gst-plugins | sctp | BSD 2-clause
gst-plugins-bad | srtp | LGPLv2.1+
gst-plugins-bad | opus | LGPLv2.1+
openwebrtc-gst-plugins | videorepair | BSD 2-clause

## OpenWebRTC Project

Project | License
:-:|:-:
openwebrtc | BSD 2-clause
openwebrtc-gst-plugins | BSD 2-clause
openwebrtc-android-sdk | BSD 2-clause
openwebrtc-ios-sdk | BSD 2-clause
openwebrtc-examples | BSD 2-clause
bowser | BSD 2-clause