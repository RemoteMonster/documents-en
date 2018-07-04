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
// 자신의 영상을 mute하기
pauseLocalVideo(bool)
// 원격의 영상을 mute하기
pauseRemoteVideo(bool)
// 자신의 음성을 mute하기
muteLocalAudio(bool)
// 원격의 영상을 mute하기
muteRemoteAudio(bool)
```

