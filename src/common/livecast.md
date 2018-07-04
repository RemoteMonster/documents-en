# Livecast

## Default Settings

Proceed with project setting for each platform before broadcasting.

## Development

The `RemonCast` class provides functions for creating and viewing broadcasts. The broadcast function can be used with the `create ()` and `join ()` functions of the `RemonCast` class.

Please refer to the following for the overall configuration and flow.

{% page-ref page="../overview/flow.md" %}

{% page-ref page="../overview/structure.md" %}

### View Registration

The caster must see himself/herself with the local view. To view the broadcast, the viewer must connect the view in which the actual video is drawn. Register the _Local View_ to let the caster to see himself/herself, and register the _Remote View_ to make the caster visible to the viewer.

```markup
<!-- Caster : local view -->
<video id="localVideo" autoplay muted></video>
```

```markup
<!-- Viewer : remote view -->
<video id="remoteVideo" autoplay></video>
```

```markup
<!-- Caster : local view -->
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
<!-- Viewer : remote view -->
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

With Interface Builder, set your view. If you have set up your preferences according to iOS - Getting Started, you have already registered your View. If you have not done yet, please refer to the following.

{% page-ref page="../ios/ios-getting-started.md" %}

Please refer to the following for the details.

{% page-ref page="../web/web-view.md" %}

{% page-ref page="../android/android-view.md" %}

{% page-ref page="../ios/ios-view.md" %}

### Broadcast Creation

You can create a broadcast using `RemonCast`\'s `create()` function. When the `create()` function is called, a broadcast channel that allows other users to connect to `Remon`\'s media server is created. At this point, a channel is created and returns its `channelId`, which allows viewers to access it.

{% tabs %}
{% tab title="iOS" %}
```swift
remonCast.create()
```

Or you can create it without _Interface Builder_ as follows.

```swift
let caster = RemonCast()
caster.serviceId = "MY_SERVICE_ID"
caster.serviceKey = "MY_SERVICE_KEY"
caster.localView = localView

remonCast.onCreate { (channelId) in
    let myChannelId = caster.channelId
}

caster.create()
```
{% endtab %}
{% endtabs %}

### Broadcast Viewing

`RemonCast`\'s `joinRoom (channelId)` function allows you to participate in the broadcast. At this time, it is necessary to inform the `channelId` of the desired channel. Usually, the user selects through the entire list by referring to the _Channel_ below.

{% tabs %}
{% tab title="Android" %}
```java
viewer = RemonCast.builder()
    .serviceId("MY_SERVICE_ID")
    .key("MY_SERVICE_KEY")
    .context(ViewerActivity.this)
    .remoteView(surfRendererRemote)        // remote video renderer
    .build();
​
viewer.onJoin(() -> {});

viewer.join("MY_CHANNEL_ID");                     // myChid from caster
```
{% endtab %}

{% tab title="iOS" %}
```swift
remonCast.join(myChannelId)                  // myChannelId from caster
```

Or you can create it without _Interface Builder_ as follows.

```swift
let viewer = RemonCast()
viewer.serviceId = "MY_SERVICE_ID"
viewer.key = "MY_SERVICE_KEY"
viewer.remoteView = remoteView

viewer.onJoin {
    // Do something
}

viewer.join("MY_CHANNEL_ID")              // myChannelId from caster
```
{% endtab %}
{% endtabs %}

### Observer

Callbacks are provided to assist in tracking various states during development.

{% tabs %}
{% tab title="Android" %}
```java
remonCast = RemonCast.builder().build();

remonCast.onInit(() -> {
    // Things to do when remon is initialized, such as UI processing, etc.
});
​
remonCast.onCreate((channelId) -> {
    // Broadcast creation and watching preparation is complete
});
​
remonCast.onJoin(() -> {
    // Start watching
});
​
remonCast.onClose(() -> {
    // End watching
});
```
{% endtab %}

{% tab title="iOS" %}
```swift
let remonCast = RemonCast()

remonCast.onInit {
    // Things to do when remon is initialized, such as UI processing, etc.
}

remonCast.onCreate { (channelId) in
    // Broadcast creation and watching preparation is complete
}

remonCast.onJoin {
    // Start watching
}

remonCast.onClose {
    // End watching
}
```
{% endtab %}
{% endtabs %}

Please refer to the following for more information.

{% page-ref page="callbacks.md" %}

### Channel

When you create a broadcast, a channel is created with a unique `channelId`. This `channelId` allows viewers to access the created broadcast. At this time, the list of all channels being broadcasted can be viewed as follows.

{% tabs %}
{% tab title="Android" %}
```java
remonCast = RemonCast.builder().build();

remonCast.featchCasts();
remonCast.onFetch((casts) -> {
    // Do something
});
```
{% endtab %}

{% tab title="iOS" %}
```swift
let remonCast = RemonCast()

remonCast.fetchCasts { (error, results) in
    // Do something
}
```
{% endtab %}
{% endtabs %}

Please refer to the following for more information.

{% page-ref page="channel.md" %}

### Termination

When all communication is finished, it is necessary to close the `RemonCast` object with `close()`. All communication resources and media stream resources are released by `close()`.

{% tabs %}
{% tab title="Android" %}
```java
remonCast = RemonCast.builder().build();
remonCast.close();
```
{% endtab %}

{% tab title="iOS" %}
```swift
let remonCast = RemonCast()
remonCast.close()
```
{% endtab %}
{% endtabs %}

### Settings

If you need more detailed settings when creating or watching a broadcast, please refer to the following.

{% page-ref page="config.md" %}

