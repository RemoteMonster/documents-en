# Web - Debug Inside

## WebRTC Internals

_webrtc-internals_ is a great tool to tackle the issues that arise from the _WebRTC_ service. If you are unfamiliar with this tool, you can run _webrtc-internals_ by open a WebRTC session from your Chrome browser, opening another tab, and typing _chrome://webrtc-internals/_ in the URL address bar.

_webrtc-internals_ can store _stats_ information about ongoing communication sessions in a large JSON file, and you can use it to look at the details as follows:

Please refer to the following for its creation.

{% embed data="{\"url\":\"https://testrtc.com/webrtc-internals-documentation/\",\"type\":\"link\",\"title\":\"The Missing chrome://webrtc-internals Documentation • testRTC\",\"description\":\"There’s a wealth of information tucked into the chrome://webrtc-internals tab, but there was up until recently very little documentation about it. So we set out to solve that, and with the assistance of Philipp Hancke wrote a series of articles on what you can find in webrtc-internals and how to make use of it. The...\",\"icon\":{\"type\":\"icon\",\"url\":\"https://testrtc.com/wp-content/uploads/2015/11/cropped-testRTC\_logo\_512x512-192x192.png\",\"width\":192,\"height\":192,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://testrtc.com/wp-content/uploads/2016/12/201612-getstats.jpg\",\"width\":799,\"height\":420,\"aspectRatio\":0.5256570713391739}}" %}

## getUserMedia Requests

The _constraints_ passed through _getUserMedia_ are recorded. The detailed information is not available, and the brief information is available.

## RTCPeerConnection

This is the most important interface to check the actual internal information.

### Overview

![From testRTC, Copyright 2018 testRTC](https://github.com/RemoteMonster/documents-en/tree/73f0c8da35a6ed76d11d15df7de8617ed1b0a140/src/.gitbook/assets/201612-webrtc-internals-structure.png)

1. It tells you how RTCPeerConnection is set, which STUN and TURN servers are used, and how its options are set.
2. The left-hand side shows a trace of _PeerConnection_ object calls. That is, the methods of the _PeerConnection_ object are listed in the order in which they are called, including its arguments \(e.g. _createOffer_\) and its callback events \(e.g. _onicecandidate_\). This is so powerful that it can be useful to check where and why ICE failures happen and to decide where you need to install a TURN server.
3. It shows the statistics received from the _Stats Tables: getStats\(\)_ method.
4. It shows graphs indicating the values of _getStats\(\)_. The _webrtc-internals_ statistics are actually a bit different from the current standards because they follow the internal format of the Chrome browser. However, these statistics are not much different and become more close to the standards gradually.

### Get stats through the code

If you want to check statistics through the code/console rather than _webrtc-internals_, you can do the following

```javascript
RTCPeerConnection.getStats(function(stats) { console.log(stats.result()); )};
```

The array values of the _RTCStatsReport_ object consists of many key-value pairs, as shown below

```javascript
RTCPeerConnection.getStats(function(stats) {
 var report = stats.result()[0];
 report.names().forEach(function(name) {
     console.log(name, report.stat(name));
 });
)}
```

One of the important principles of reading these Report objects is that the _key_ name ending with an _Id_ usually points to the _id_ attribute of another report object. Therefore, the structure shows that almost all report objects are linked together. In addition, remember that most values are represented as strings.

The most important attribute of _RTCStatsReport_ is the _type_ of each _report_. Here are some of the important ones.

* googTrack
* googLibjingleSession
* googCertificate
* googComponent
* googCandidatePair
* localCandidate
* remoteCandidate
* ssrc
* VideoBWE

Let\'s look at these reports one by one.

### googCertificate report

The _googCertificate report_ contains information about the _DTLS certificate_ used by the local side and used for the certificate itself. You can find out more about this in the [RTCCertificateStats dictionary](https://w3c.github.io/webrtc-stats/#certificatestats-dict).

### googComponent report

The _googComponent_ report acts as an adhesive between the certificate statistics and the connection. That is, it contains a link to the currently active candidate pair.

### googCandidatePair report

A _googCandidatePair_ report covers a pair of ICE Candidates. This report provides the following information:

* The total number of packets and bytes sent and received \(_bytesSent, bytesReceived, packetsSent; packetsReceived_ is missing for unknown reason\). By default, these are raw UDP or TCP bytes, including RTP headers.
* The _googActiveConnection_ entry tells you whether the connection is currently active. Most of the time, you\'ll only be interested in the statistics of the active candidate pair. You can find more information about this [here](https://w3c.github.io/webrtc-stats/#transportstats-dict).
* The number of STUN requests sent and responses received \(_requestsSent, responsesReceived, requestsReceived, responsesSent_\). That is, the _count_ indicates the number of incoming / outgoing STUN requests used in the ICE process.
* You can the round trip time of the last STUN request with _googRtt_. This is different from _googRtt_ in _ssrc report_.
* _localCandidateId_ and _remoteCandidateId_ indicate the _id_s of the _localCandidate_ and _remoteCandidate_ objects.
* _googTransportType_ specifies the transfer type. It will usually be _udp_ in most cases, but it can be _TURN over TCP_ when a TURN server is used. When [ICE-TCP](https://webrtcglossary.com/ice-tcp/) is used, it will be set to _tcp_.

### localCandidate, remoteCandidate report

The _localCandidate_ and _remoteCandidate_ report allow you to check the ip address, port number and type of the candidate.

### Ssrc report

The _ssrc_ report is one of the most important ones. There is one for each audio or video track sent and received via the _peerconnection_. The standard separately defines it as [_MediaStreamTrackStats_](https://w3c.github.io/webrtc-stats/#mststats-dict) and [_RTPStreamStats_](https://w3c.github.io/webrtc-stats/#streamstats-dict). This report is affected by whether the media type is audio or video, whether it is sent or received. Let\'s start with the common values.

* _mediaType_: tells whether the media type is audio or video.
* ssrc: indicates a unique value such as whether media is sent or received.
* _googTrackId_: indicates the id of the track to which these statistics applies. This can also be found in the local or remote media stream tracks in SDP. In principle, anything with an _Id_ at the end should point to a different report, but this is an exception.
* _googRtt_: indicates the _round-trip time_, which is measured from _RTCP_.
* _transportId_: indicates the component used to transmit this RTP stream. When Bundle is used, the same value will be assigned to both audio and video streams.
* _googCodecName_: specifies the name of the codec: _opus, VP8, VP9, ​​H264,_ etc. You can also check the codec implementation with _codecImplementationName stat_.
* _bytesSent, bytesReceived, packetsSent, packetsReceived_ \(depends on whether the _ssrc_ indicates sending or receiving\): allow you to calculate _bitrates_. Since these numbers are cumulative, you must make the appropriate calculations with the time taken since the previous measurements made with _getStats_. The [sample](http://w3c.github.io/webrtc-pc/#example) code in the standard is quite nice, but keep in mind that you might end up with negative rates because Chrome sometimes resets these counter values.
* _packetsLost_: indicates the number of lost packets. It is collected via RTCP for the sender and locally collected for the receiver. This is one of the most important values ​​indicating call quality.

### Voice

* For audio tracks, there are _audioInputLevel_ and _audioOutputLevel_ \(these are called [audioLevel](https://w3c.github.io/webrtc-stats/#dom-rtcmediastreamtrackstats-audiolevel) in the standard\). These values tell whether an audio signal comes from the microphone or the speaker. These are used to detect the Chrome [Audio Processing Bugs](https://bugs.chromium.org/p/webrtc/issues/detail?id=4799).
* _googJitterReceived_ and _googJitterBufferReceived_: provide information about the [amount of jitter received](https://webrtcglossary.com/jitter/) and the [jitter buffer state](https://webrtcglossary.com/jitter-buffer/).

### Video

* _googNacksSent_: information about the number of [NACK](https://webrtcglossary.com/nack/) packets
* _googPLIsSent_: information about the number of [PLI](https://webrtcglossary.com/pli/) packets
* _googFIRsSent_: Information about the number of [FIR](https://webrtcglossary.com/fir/) packets
* The above information helps you understand the impact of _packet loss_ on video quality.
* _googFrameWidthInput, googFrameHeightInput, googFrameRateInput_: indicate the input frame size and rate.
* _googFrameWidthSent, googFrameHeightSent, googFrameRateSent_: indicate the frame size and rate sent over the physical network.
* _googFrameWidthReceived, googFrameHeightReceived_: indicate the received frame size.
* _googFrameRateReceived, googFrameRateDecoded, googFrameRateOutput_: indicate the frame rate received.
* On the video encoder side, you can see the difference between these values ​​and see why the resolution of the image is lowered. Typically, this is usually caused by insufficient CPU or bandwidth.
* The lowered frame rate can be observed by comparing _googFrameRateInput_ and _googFrameRateSent_. In addition, you can see if the lowered resolution is caused by the CPU \(_googCpuLimitedResolution_ is _true_\) or insufficient bandwidth \(_googBandwidthLimitedResolution_ is _true_\) Information. Whenever any of these conditions changes, the _googAdaptionChanges_ counter will be incremented.
* We have artificially caused packet loss. On the response side, Chrome tries to lower the resolution at t = 184, where _googFrameWidthSent_ and _googFrameWidthInput_ become different. At t = 186, you can see that the input frame rate changes from _30fps_ to near _zero \(0\)_.

### VideoBWE report

The _VideoBWE_ report contains the bandwidth estimate. The following information can be found in the report.

* _googAvailableReceiveBandwidth_: The bandwidth available for the video data being received.
* _googAvailableSendBandwidth_: The available bandwidth for the video data being transmitted.
* _googTargetEncBitrate_: The target bitrate of the video encoder. This tries to maximize the use of available bandwidth.
* _googActualEncBitrate_: The actual bitrate coming out of the video encoder. This should match the target bitrate.
* _googTransmitBitrate_: The actual bitrate of transmission. If this is significantly different from _googActualEncBitrate_, this is probably due to [_forward error correction_](https://webrtcglossary.com/fec/).
* _googRetransmitBitrate_: This allows measuring the bitrate of retransmission if RTX is used. This is useful for measuring packet loss.
* _googBucketDelay_: A measure of Google\'s \"leaky bucket\" strategy associated with large frames. It does not matter much.

