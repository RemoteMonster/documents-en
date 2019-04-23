# Communication

## Default settings <a id="undefined"></a>

Proceed with project setting for each platform before communication.

## Development <a id="undefined-1"></a>

The `RemonCll` class provides functions for communication. The communication function can be used with the `connect()` function of the `RemonCall` class.

Please refer to the following for the overall configuration and flow

{% page-ref page="../overview/flow.md" %}

{% page-ref page="../overview/structure.md" %}

### View 등록

To view the communication, the viewer must connect the view in which the actual video is drawn. Register the _Local View_  to see himself/herself, and register the _Remote View_ to make the other.

{% tabs %}
{% tab title="Web" %}
```javascript
<!-- local view -->
<video id="localVideo" autoplay muted></video>
<!-- remote view -->
<video id="remoteVideo" autoplay></video>
```
{% endtab %}

{% tab title="Android" %}
```markup
<!-- local view -->
<com.remotemonster.sdk.PercentFrameLayout
    android:id="@+id/perFrameLocal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <org.webrtc.SurfaceViewRenderer
        android:id="@+id/surfRendererLocal"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</com.remotemonster.sdk.PercentFrameLayout>
```

```markup
<!-- remote view -->
<com.remotemonster.sdk.PercentFrameLayout
    android:id="@+id/perFrameRemote"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <org.webrtc.SurfaceViewRenderer
        android:id="@+id/surfRendererRemote"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</com.remotemonster.sdk.PercentFrameLayout>
```
{% endtab %}

{% tab title="iOS" %}
With Interface Builder, set your view. If you have set up your preferences according to iOS - Getting Started, you have already registered your View. If you have not done yet, please refer to the following.

{% page-ref page="../ios/ios-getting-started.md" %}
{% endtab %}
{% endtabs %}

Please refer to the following for the details.

{% page-ref page="../web/web-view.md" %}

{% page-ref page="../android/android-view.md" %}

{% page-ref page="../ios/ios-view.md" %}

### Make a call

You can create a communication using `RemonCast`'s `connectChannel()` function. When the `connectChannel()` function is called, a channel that allows other users to connect to `Remon`'s media server is created. At this point, a channel is created and returns its `channelId`, which allows the other to access it.

{% tabs %}
{% tab title="Web" %}
```javascript
// <video id="localVideo" autoplay muted></video>
// <video id="remoteVideo" autoplay></video>
let myChid
​
const config = {
  credential: {
    serviceId: 'MY_SERVICE_ID',
    key: 'MY_SERVICE_KEY'
  },
  view: {
    local: '#localVideo',
    remote: '#remoteVideo'
  }
}
​
const listener = {
  onConnect(channelId) {
    myChannelId = channelId
  },
  onComplete() {
    // Do something
  }
}
​
const caller = new Remon({ listener, config })
caller.connectCall()
```
{% endtab %}

{% tab title="Android" %}
```java
caller = RemonCall.builder()
    .serviceId("MY_SERVICE_ID")
    .key("MY_SERVICE_KEY")
    .context(CallActivity.this)
    .localView(surfRendererLocal)
    .remoteView(surfRendererRemote)
    .build();
​
caller.onConnect((channelId) -> {
    myChannelId = channelId  // Callee need chid from Caller for connect
});
​
caller.onComplete(() -> {
    // Caller-Callee connect each other. Do something
});
​
caller.connect();
```
{% endtab %}

{% tab title="Swift" %}
```objectivec
let caller = RemonCall()

caller.onConnect { (channelId) in
    let myChannelId = channelId          // Callee need channelId from Caller for connect
}

caller.onComplete {
    // Caller-Callee connect each other. Do something
}

caller.connect()
```
{% endtab %}

{% tab title="Objc" %}
```swift
RemonCall *caller = [[RemonCall alloc] init];

[caller onConnectWithBlock:^(NSString * _Nullable channelId) {
// Callee need channelId from Caller for connect
    [self setMyChannelId:channelId];
}];

[caller onCompleteWithBlock:^{
    // Caller-Callee connect each other. Do something
}];

[caller connect:chId :nil];
```
{% endtab %}
{% endtabs %}

### Get a call <a id="undefined-3"></a>

`RemonCall`'s `connectChannel(channelId)` function allows you to participate in the communication. At this time, it is necessary to inform the `channelId` of the desired channel.

{% tabs %}
{% tab title="Web" %}
```javascript
// <video id="localVideo" autoplay muted></video>
// <video id="remoteVideo" autoplay></video>
const config = {
  credential: {
    serviceId: 'MY_SERVICE_ID',
    key: 'MY_SERVICE_KEY'
  },
  view: {
    local: '#localVideo',
    remote: '#remoteVideo'
  }
}
​
const listener = {
  onComplete() {
    // Do something
  }
}
​
const callee = new Remon({ listener, config })
callee.connectCall('MY_CHANNEL_ID')
```
{% endtab %}

{% tab title="Android" %}
```java
callee = RemonCall.builder()
    .serviceId("MY_SERVICE_ID")
    .key("MY_SERVICE_KEY")
    .context(CallActivity.this)
    .localView(surfRendererLocal)
    .remoteView(surfRendererRemote)
    .build();

callee.onComplete(() -> {
    // Caller-Callee connect each other. Do something
});

callee.connect("MY_CHANNEL_ID");
```
{% endtab %}

{% tab title="Swift" %}
```swift
let callee = RemonCall()

callee.onComplete {
    // Caller-Callee connect each other. Do something
}

callee.connect("MY_CHANNEL_ID")
```
{% endtab %}

{% tab title="Objc" %}
```swift
RemonCall *callee = [[RemonCall alloc] init];

[callee onCompleteWithBlock:^{
    // Caller-Callee connect each other. Do something
}];

[callee connect:chId :nil];
```
{% endtab %}
{% endtabs %}

### Callbacks <a id="observer"></a>

Callbacks are provided to assist in tracking various states during development.

{% tabs %}
{% tab title="Web" %}
```javascript
const listener = {
  onInit(token) {
    // Things to do when remon is initialized, such as UI processing, etc.
  },
​  
  onConnect(channelId) {
    // Make a call then wait the callee
  },
​
  onComplete() {
    // Start between Caller and Callee
  },
​  
  onClose() {
    // End calling
  }
}
```
{% endtab %}

{% tab title="Android" %}
```java
remonCall = RemonCall.builder().build();

remonCall.onInit((token) -> {
    // Things to do when remon is initialized, such as UI processing, etc.
});
​
remonCall.onConnect((channelId) -> {
    // Make a call then wait the callee
});
​
remonCall.onComplete(() -> {
    // Start between Caller and Callee
});
​
remonCall.onClose(() -> {
    // End calling
});
```
{% endtab %}

{% tab title="Swift" %}
```swift
let remonCall = RemonCall()

remonCall.onInit { (token) in
    // Things to do when remon is initialized, such as UI processing, etc.
}
​
remonCall.onConnect { (channelId) in
    // Make a call then wait the callee
}
​
remonCall.onComplete {
    // Start between Caller and Callee
}
​
remonCast.onClose {
    // End calling
}
```
{% endtab %}

{% tab title="Objc" %}
```objectivec
RemonCall *remonCall = [[RemonCall alloc] init];

[remonCall onInitWithBlock:^{
    // Things to do when remon is initialized, such as UI processing, etc.
}];

[remonCallter onConnectWithBlock:^(NSString * _Nullable chId) {
    // Make a call then wait the callee
}];

[remonCall onCompleteWithBlock:^{
    // Start between Caller and Callee
}];

[remonCall onCloseWithBlock:^{
    // End calling
}];
```
{% endtab %}
{% endtabs %}

Please refer to the following for more information.​

{% page-ref page="callbacks.md" %}

### Channel <a id="channels"></a>

When you create a communication, a channel is created with a unique `channelId`. This `channelId` allows the other to access the created communication. At this time, the list of all channels being communication can be viewed as follows.

{% tabs %}
{% tab title="Web" %}
```javascript
const remonCall = new Remon()
const calls = await remonCall.fetchCalls()
```
{% endtab %}

{% tab title="Android" %}
```java
remonCall = RemonCall.builder().build();

remonCall.fetchCalls();
remonCall.onFetch(calls -> {
    // Do something
});
```
{% endtab %}

{% tab title="Swift" %}
```swift
let remonCall = RemonCall()

remonCall.fetchCalls { (error, results) in
    // Do something
}
```
{% endtab %}

{% tab title="Objc" %}
```objectivec
RemonCall *remonCall = [[RemonCall alloc]init];
[remonCall fetchCastsWithIsTest:YES
              complete:^(NSArray<RemonSearchResult *> * _Nullable chs) {
                            // Do something
}];
```
{% endtab %}
{% endtabs %}

Please refer to the following for more information.

{% page-ref page="channel.md" %}

### Termination <a id="undefined-4"></a>

When all communication is finished, it is necessary to close the `RemonCall` object with `close()`. All communication resources and media stream resources are released by `close()`.

{% tabs %}
{% tab title="Web" %}
```javascript
const remonCast = new Remon()
remonCast.close()
```
{% endtab %}

{% tab title="Android" %}
```java
remonCall = RemonCall.builder().build();
remonCall.close();
```
{% endtab %}

{% tab title="Swift" %}
```swift
let remonCall = RemonCall()
remonCall.close()
```
{% endtab %}

{% tab title="Objc" %}
```objectivec
RemonCall *remonCall = [[RemonCall alloc]init];
[remonCall closeRemon:YES];
```
{% endtab %}
{% endtabs %}

### Setting <a id="undefined-5"></a>

If you need more detailed settings when creating or watching a communication, please refer to the following.

{% page-ref page="config.md" %}

