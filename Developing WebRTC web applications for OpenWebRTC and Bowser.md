The WebRTC API is not yet finalised and part of the reason for OpenWebRTC's existence is to be an interoperable implementation. As with any other web API, small differences in how the APIs work in the respective browsers can exist.

This page hopes to document them to make your life easier when writing web code to work with other popular browsers and OpenWebRTC (in the form of [Bowser](https://github.com/EricssonResearch/bowser), the `openwebrtc-daemon` or, for example, an [iOS hybrid app](https://github.com/EricssonResearch/openwebrtc-examples/wiki/Developing-a-hybrid-iOS-app).)

## Bowser-specific Notes
**For Bowser-specific notes, [see here](https://github.com/EricssonResearch/bowser/wiki/Developing-for-Bowser)**

## WebRTC API and prefixes
Where needed the `webkit` prefix is used for WebRTC API's. Examples of top level WebRTC API's:
```
navigator.webkitGetUserMedia = getUserMedia;
webkitMediaStream = MediaStream;
webkitRTCPeerConnection = RTCPeerConnection;
RTCSessionDescription
RTCIceCandidate
```
All API's are defined in [webrtc.js](https://github.com/EricssonResearch/openwebrtc/blob/master/bridge/client/webrtc.js) if you want to check the source.

## General Notes

* **WebRTC classes may not exist until after the page load has finished** - This is because the page needs to download and execute `owr.js`. You need to be aware of this if you check for webrtc support on page load.

## Reference application
If you get stuck we also provide a [reference WebRTC application](https://github.com/EricssonResearch/openwebrtc-examples/tree/master/web) that works in Bowser/OpenWebRTC, Chrome and Firefox.

## Debugging
The preferred method for debugging WebRTC web applications quickly is to run the web application in Firefox or OS X Safari, along with the [OpenWebRTC daemon and browser extension](https://github.com/EricssonResearch/openwebrtc-browser-extensions). This is particularly useful to debug issues related to call setup and signalling.