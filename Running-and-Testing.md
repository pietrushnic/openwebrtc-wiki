If you're looking at this, you've hopefully [[built everything|Building-OpenWebRTC]] and you're itching to start using OpenWebRTC. There are a few possibilities for getting started actually testing and using OpenWebRTC...

**Once you're set up, take a look at [[the test app page|WebRTC-Test-Apps]] for some links to WebRTC test apps you can use to try it all out.**

# The daemon

With the daemon method, you can use OpenWebRTC with any existing browser. The video rendering performance will be suboptimal with this setup, but it is very useful for testing purposes to be able to use entirely standard web browsers.

## Step 1 - Installing a Browser Extension

### Safari
Currently we only provide a Safari extension, just [download](https://github.com/EricssonResearch/openwebrtc-browser-extensions/raw/master/safari/OpenWebRTC.safariextz) and double-click the downloaded file to install in Safari.

![Safari gUM screenshot](https://github.com/EricssonResearch/openwebrtc-browser-extensions/blob/master/imgs/safari_gum.png)

If you want to modify the extension you need to use the Extension Builder in Safari:

![Safari screenshot](https://github.com/EricssonResearch/openwebrtc-browser-extensions/blob/master/imgs/safari_screenshot.png)

### Firefox and Chrome

Even though Firefox and Chrome already have WebRTC support, you may want to use OpenWebRTC as a backend. You should be able to use [Greasemonkey](https://github.com/greasemonkey/greasemonkey/) with Firefox or [Tampermonkey](http://tampermonkey.net) with Chrome to load a user script and quite easily inject code like this:

```
var script = document.createElement("script");
script.src = "http://localhost:10717/owr.js";
document.head.appendChild(script);
```

## Step 2 - Running the daemon

OpenWebRTC browser extensions communicate with the *daemon* via its local web server. Before any page load, extensions fetch JavaScript from the daemon containing WebRTC functionality and inject it in to the current context. The daemon runs in a separate process in the backgound.

![Daemon screenshot](https://github.com/EricssonResearch/openwebrtc-browser-extensions/blob/master/imgs/daemon.png)

Start the daemon by running the following command from the root directory of your openwebrtc clone:
```
./out/x86_64-apple-darwin/bin/daemon
```

Note that you need to [[build|Building-OpenWebRTC]] OpenWebRTC first. In this example we run it on OS X, but the daemon is also available on Linux.

# Bowser on iOS

We also released [Bowser for iOS](https://github.com/EricssonResearch/bowser) which is a web browser for iOS with WebRTC support using OpenWebRTC. At the time of release, it works almost identically to the daemon method discussed above, just integrated into an iOS app using a UIWebView to provide the web browsing functionality.

# Your own application!

We hope the framework is also useful for development of your own applications or for integration into web browsers. Please let us know if you're using OpenWebRTC and we can perhaps link to your project here! :smiley: