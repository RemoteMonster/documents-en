# Web - View

## Basic

```markup
<video id="remoteVideo" autoplay controls></video>
<video id="localVideo" autoplay controls muted></video>
```

If you add the _Controls_ attribute, you can add video controls.

For _Local Video_, you usually need to add the _muted_ attribute to eliminate the howling of your voice again.

## Advanced

```javascript
// Mute your own local video
pauseLocalVideo(bool)
// Mute the remote video
pauseRemoteVideo(bool)
// Mute the your own local audio and mic stream
muteLocalAudio(bool)
// Mute the remote audio stream
muteRemoteAudio(bool)
```

