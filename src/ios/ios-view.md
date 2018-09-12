# iOS - View

With _Interface Builder_, you can use _StoryBoards_ for configuration and development. You can configure a View with your individual code through the provided SDK.

For information on how to use _Interface Builder_, refer to the following.

If you don't use _Interface Builder_, refer to the following code.

{% tabs %}
{% tab title="Swift" %}
```swift
let remonCall = RemonCall()
remonCall.remoteView = myRemoteView
remonCall.localView = myLocalView
```
{% endtab %}

{% tab title="Objc" %}
```objectivec
RemonCall *remonCall = [[RemonCall alloc] init];
remonCall.remoteView = myRemoteView;
remonCall.localView = myLocalView;
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you specify a `remoteView` or `localView` in `RemonController`, `RemonController` will add the video rendering view to the specified view and track the changes in the specified view to set the video rendering view suitable for the size of the specified view.
{% endhint %}

