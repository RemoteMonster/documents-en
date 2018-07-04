# iOS - Getting Started

## iOS - Getting Started

### Prerequisites

* Xcode IDE
* iOS 9.2 or later

### Creating and Configuring a New Project

Create a new _Swift_-based project in Xcode.

You need to set `No` to the `Enable Bitcode` option in the `Build Settings` after project creation.

![Bitcode](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcodpkrjb5mf2cia-ios_bitcode-1.png)

You also need to add or change the following items in `Info.plist`.

![Settings](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcoiwhlabmetgwro-ios_buildsettings.png)

### SDK Installation - Cocoapods

Add `pod 'RemoteMonster', '~> 2.0'` to the `Podfile` of the project in which you want to install the SDK.

{% code-tabs %}
{% code-tabs-item title="Podfile" %}
```text
target 'MyApp' do
  pod 'RemoteMonster', '~> 2.0'
end
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then run `pod install` on the terminal. If `pod install` does not work, run `pod update`.

```bash
$ pod install
```

### SDK Installation - Direct Import

First, download the latest version of the iOS SDK via the link below.

{% embed data="{\"url\":\"https://github.com/remotemonster/ios-sdk\",\"type\":\"link\",\"title\":\"RemoteMonster/ios-sdk\",\"description\":\"ios-sdk - RemoteMonster iOS SDK & examples\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars2.githubusercontent.com/u/20677626?s=400&v=4\",\"width\":400,\"height\":400,\"aspectRatio\":1}}" %}

There are two frameworks when I unzip the downloaded _RemoteMonster iOS SDK_. Drag and drop each _Framework_ from the _Finder_ to the _Project Tree_ pane. You will then see the _RemoteMonster SDK_ recognized as a framework.

![Framework](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcooludschostsi5-ios_importframework-2.png)

### Remon Setup and Layout Configuration

With `InterfaceBuilder`, `Remon` can be configured by using `RemonIBController`.

* Add a subobject of RemonIBController \(RemonCall or RemonCast\) to the storyboard.
  * `RemonCall` supports 1:1 communication and `RemonCast` supports 1: N broadcasting.
  * Use `Utilities` view to configure `Remon` in _InterfaceBuilder_.
* Set `ServiceID` and `Service Key`.
  * If you want to run a test simply, you do not need to enter anything.
  * If you consider actual service, please refer to the following and get the service key to use.

{% page-ref page="../common/service-key.md" %}

![](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcoxn8ud-p0oah4p-basic_config.png)

* Place `View` at the desired location in the desired scene in the storyboard, and bind it to the `remoteView` and `localView` of the `RemonIBController`.

![](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcoz_-wy1mziw7yj-basic_config3-2.png)

* Import the _RemoteMonster SDK_ into the `ViewContoller` using `Remon`, and bind the `RemonIBController` object to an outlet variable.

![](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcoapvrxmvhnz1qa-config3.png)

### Development

Now you are ready for development. Refer to the following for detailed development methods.

#### Broadcast

`RemonCast` can make broadcasting functions easy and fast.

**Broadcast Transmission**

```swift
let caster = RemonCast()
caster.create()
```

**Broadcast Viewing**

```swift
let viewer = RemonCast()
viewer.join("CHANNEL_ID")
```

Or refer to the following for more details.

{% page-ref page="../common/livecast.md" %}

#### Communication

`RemonCall` can make communication functions easy and fast

```swift
let remonCall = RemonCall()
remonCall.connect("CHANNEL_ID")            // Communication
```

Or refer to the following for more details.

{% page-ref page="../common/communication.md" %}

### Known Caveats

#### Audio Types

There are two types of audio in _Remon_: _voice_ and _music_. The default operation mode is _voice_, and you can use the _music_ mode if you want to use various sounds rather than voice. In particular, the _music_ mode tends to be more suitable for the broadcast.

![](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcoe6ox4dngv5kwc-remonsettings.png)

Add the `RemonSettings.plist` file to your project and change the _AudioType_ value to the desired mode.

#### Background Mode Support

If you need to constantly connect to the SDK in the background, you can set the option in the following menu: _Project&gt; Targets&gt; Capabilities&gt; Background Modes_. If you do not set the background mode, the connection with _RemoteMonster_ will be terminated and the broadcast and call will be ended when the app moves to the background.

![](../.gitbook/assets/assets-lalxanhbadmg35tjnme-lguxznufvictum8-kvv-lguxcogoamcofl2apdr-2018-06-01-10.36.28.png)

Please refer to the following for background mode support.

{% embed data="{\"url\":\"https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html\",\"type\":\"link\",\"title\":\"Background Execution\",\"description\":\"Introduces iOS and describes the development process for iOS applications.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://developer.apple.com/favicon.ico\",\"aspectRatio\":0}}" %}

