# Error Code

## Overview

The _RemoteMonster_ APIs have two major exceptions: _Fail_ and _Error_.

### Fail

_Fail_ refers to an exception that mainly occurs during communication. _Fail_ occurs when the communication connection is not established, when the connection is disconnected, or when the connection is unstable. You can receive Fail events with the `onStateChange` callback method.

### Error

_Error_ refers to an exception in a broader area, including _Fail_. You will receive an Error code through the `onError` callback method.

#### InvalidParameterError

* When a parameter given for `new Remon` is invalid
  * If there is no _Key, Service Id, Local View, Remote View_, or if there is no `config` or _Callback_, or if the parameter is too long
* An incorrect value is given at connectChannel \(if the length is less than 1 or too long\(100 or more\)\)
* `UnsupportedPlatformError`
  * If the Browser does not support
  * If the Version does not support

#### InitFailedError

* There is an error when returning a RESTful API
  * 500 Error
* The signaling server is not operating
  * Since the web server is running, it delivers the wrong page
* The web server is not operating
  * 400 Error
* There is a problem with the _Web Socket_ and _RESTful_ host
* If an error occurs during the _Web Socket_ startup

#### WebSocketError: websocket communication errors

* Send Error
* Receive Error

#### ConnectChannelFailedError

* There is no channel information in the return of _create / connect_.
* The server will automatically switch to _onCreateChannel_ if there is an attempt to connect when the _channel_ expires or when there is no channel.

#### BusyChannelError

* The channel is already in use

#### UserMediaDeviceError

* The media has not been imported, especially Camera \(even though video is turned on\)\)
* The _Video Capture_ has not been imported.

#### ICE\(Failed\)Error

* A _peerConnection_ cannot be created
* The SDP already exists, but its own SDP is created additionally
* There is an _ICE_ /_SDP_ parsing or addition failure

