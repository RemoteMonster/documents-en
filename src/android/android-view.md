# Android - View

## Introduction

We provide two most important classes for layout: `org.webrtc.SurfaceViewRender` which is a _View_ used for image output. and `PercentFrameLayout` which helps to efficiently lay out `the SurfaceViewRender` in `RelativeLayout`. Given these classes, `SurfaceViewRender` is the most important; therefore, let\'s first look at it.

## SurfaceViewRender

### Basic

You can use _SurfaceViewRender_ by placing it in the layout in your Android layout file like this:

```markup
<RelativeLayout
android:layout_width="match_parent"
android:layout_height="match_parent"
android:background="@android:color/darker_gray"
android:layout_alignParentBottom="false"
android:layout_weight="2">
  <org.webrtc.SurfaceViewRenderer
    android:id="@+id/remote_video_view"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
  <org.webrtc.SurfaceViewRenderer
    android:id="@+id/local_video_view"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
</RelativeLayout>
```

Then, the object of this _View_ is obtained in a specific _Activity_ as follows.

```java
SurfaceViewRender localRender =
  (SurfaceViewRenderer) findViewById(R.id.local_video_view);
SurfaceViewRender remoteRender =
  (SurfaceViewRenderer) findViewById(R.id.remote_video_view);
```

You are now ready to use this _View_. If you set these two pairs of _Views_ in the `Config` object when you create an object of class `Remon`, the communication will start and the camera or remote video stream will be output to the _View_.

### Advenced

Let\'s look at some `SurfaceViewRender` methods.

You can set the Z-Order between views. That is, set a view above other views. If there are overlapping views, the other views should be `false` and only the view should be set to `true`.

```java
localRender.setZOrderMediaOverlay(true/false);
```

You can show the image of the view in a mirror-effect manner.

```java
localRender.setMirror(false);
```

Determines how to fill the layout.

```java
localRender.setScalingType(RendererCommon.ScalingType);
```

## PercentFrameLayout

_RemoteMonster Android SDK_ provides `PercentFrameLayout` for easy layout of video views. `PercentFrameLayout` allows you to freely position and dynamically move video views within `RelativeLayout`. `PercentFrameLayout` places the views in the layout in a percentage manner.

Places the layout with 100% width and height on _relativeLayout_.

```java
layout.setPosition(0,0,100,100);
```

Divide _relativeLayout_ into four quarters and place the layout to take 50% of the available space in the bottom left corner.

```java
layout.setPosition(0,50,50,50);
```

