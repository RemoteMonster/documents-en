# Flow

## Overview

There is a common trend in the overall use of _RemoteMonster_. Please refer to the following for the details

## Livecast

The broadcasting service is divided into the following cases: simply creating a room to transmit a broadcast and connecting to a room to receive a broadcast. It has the following flow.

* Caster: the party transmitting a broadcast
* Viewer: the party watching the broadcast

|  | Initialization | Channel Creation | Channel Connection | Termination |
| --- | --- | --- | --- | --- |
| Caster Event | ready RemoteMonster | `create()` | - | `close()`, disconnect |
| Caster Callback | `onInit` | `onCreate` | - | `onClose` |
| Viewer Event | ready RemoteMonster | - | `join('channelId')` | `cloase()`, disconnect |
| Viewer Callback | `onInit` | - | `onJoin` | `onClose` |

## Communication

The communication service is divided into the following acts: requesting a call and receiving a call. The calling party creates a channel and waits for the other party. At this time, the other party is connected to the same channel through the obtained _chid_, and mutual communication is started.

* Caller : the party making a communication request
* Callee : the party responding to the communication request

|  | Initialization | Channel Creation | Channel Connection | Termination |  |
| --- | --- | --- | --- | --- |
| Caller Event | ready RemoteMonster | `connect()` | Wait callee | Caller, Callee Connected | `close()`, disconnect |
| Caller Callback | `onInit` | `onConnect` | - | `onComplete` | `onClose` |
| Callee Event | ready RemoteMonster | - | `connect('channelId')` | Caller, Callee Connected | `close()`, disconnect |
| Callee Callback | `onInit` | - | `onConnect` | `onComplete` | `onClose` |

