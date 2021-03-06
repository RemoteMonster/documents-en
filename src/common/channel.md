# Channel

## Overview

_RemoteMonster_ provides resources shared by users during broadcasting and communication under the name channel. This channel is created the first time you create it, provides each unique Id, and gets or retrieves a list of them to connect to a specific channel. In addition, you can assign a name to your channel in order to make it easier to use.

|  | Class | Id\(unique\) | Name | Methods | Callbacks |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Livecast | remonCast | ChannelId | ChannelName | `create`, `join`, `fetchCasts` | `onCreate`, `onJoin` |
| Communication | remonCall | ChannelId | ChannelName | `connect`, `fetchCalls` | `onConnect`, `onComplete` |

Please refer to the following for the overall flow and corresponding _Callback_s.

{% page-ref page="../overview/flow.md" %}

{% page-ref page="callbacks.md" %}

## Livecast

Here is how to get a list of broadcasts on the air. It is commonly used in UIs to find broadcasts to enter from the list.

{% tabs %}
{% tab title="Web" %}
```javascript
const remonCast = new Remon()
const casts = await remonCast.fetchCasts()    // Return Promise
```
{% endtab %}

{% tab title="Android" %}
```java
remonCast = RemonCast.builder().context(ListActivity.this).build();
remonCast.fetchCasts();
remonCast.onFetch(casts -> {
    for (Channel cast : casts) {
        myChannelId = cast.getId;
    }
});

remonCast.join(myChannelId);
```
{% endtab %}

{% tab title="Swift" %}
```swift
remonCast.fetchCasts { (err, results) in
    if let err = err {
            // If there is an error during the search, remonCast.onError () will not be called.
        print(err.localizedDescription)
    } else if let results = results {
        for cast:RemonSearchResult in results {
            // Do Somethig
        }
    }
}

remonCast.join(myChannelId)
```
{% endtab %}

{% tab title="Objc" %}
```objectivec
RemonCast *remonCast = [[RemonCast alloc]init];
[remonCast fetchCastsWithIsTest:YES complete:^(NSArray<RemonSearchResult *> * _Nullable chs) {
     if (chs != nil) {
          for (RemonSearchResult *item in chs) {
               // Do Somethig                         
           }
     }                       
}];
```
{% endtab %}
{% endtabs %}

## Communication

Here's how to get a list of calls from your communications. It is used in situations such as a random chat and is not generally used.

{% tabs %}
{% tab title="Web" %}
```javascript
const remonCall = new Remon()
const calls = await remonCall
  .fetchCasts()                             // Return Promise
  .filter(item => item.status === "WAIT")
```
{% endtab %}

{% tab title="Android" %}
```java
remonCall = RemonCall.builder().build();
remonCall.fetchCalls();
remonCall.onFetch(calls -> {
    for (Channel call : calls) {
        if (call.getStatus.equals("WAIT")) {   // Only WAIT channels
            myChannelId = call.getId;
        }
    }
});

remonCall.connect(myChannelId)
```
{% endtab %}

{% tab title="Swift" %}
```swift
let remonCall = RemonCall()
remonCall.fetchCalls { (err, results) in
    if let err = err {
            // If there is an error during the search, remonCall.onError() will not be called.
        print(err.localizedDescription)
    } else if let results = results {
        for call:RemonSearchResult in results {
            if itme.status == "WAIT" {        // Only WAIT channels
                // Do Somethig
            }
        }
    }
}

remonCall.connect(myChannelId)
```
{% endtab %}

{% tab title="Objc" %}
```objectivec
RemonCall *remonCall = [[RemonCall alloc]init];
[remonCall fetchCastsWithIsTest:YES complete:^(NSArray<RemonSearchResult *> * _Nullable chs) {
     if (chs != nil) {
          for (RemonSearchResult *item in chs) {
               if ([item.status isEqualToString@"WAIT"]) {
                    // Do Somethig
               }                  
           }          
     }                       
}];
```
{% endtab %}
{% endtabs %}

