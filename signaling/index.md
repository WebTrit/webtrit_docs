# WebTrit Signaling Protocol

The **WebTrit Signaling Protocol (WTSP)** is designed to establish a reliable, predictable, and [self-recoverable mechanism](features/self_recoverable_mechanism.md) for connecting **WebTrit apps** to the **WebTrit Core**.

WTSP utilizes WebSocket as its communication protocol, mandating the specification of the sub-protocol as [`webtrit-protocol`](websocket_subprotocol.md).

WTSP URL structure:
```
wss://<WebTrit Core host>[:<WebTrit Core port>]/signaling/v1?token=<token>[&force=<true | false (by default)>]
```
where:
- `token` - a session token obtained from the [WebTrit Core API](../api/index.md#core)
- `force` - a flag to define, must current connection close the active one if it exists (`true`) or not (`false`)

WTSP message data is a string serialized in the JSON format.

## Messages

WTSP classifies messages into the following categories:
- `handshake`
  - Direction: *server to client* and *client to server*.
  - Purpose: to maintain the current connection with an active client session.
- `request`
  - Direction: *client to server*.
  - Purpose: to execute an action.
  - Logically divided into three groups:
    - _Session_
    - _Line_
    - _Call_
- `response`
  - Direction: *server to client*.
  - Purpose: to acknowledge a received `request`.
- `event`
  - Direction: *server to client*.
  - Purpose: to notify about the progress of action execution and external events.
  - Logically divided into three groups:
    - _Session_
    - _Line_
    - _Call_

### Handshake

Upon successful WebSocket connection, the **WebTrit Core** server sends the current session's [handshake `state`](handshake/state.md). Clients should only send requests after receiving this message; failing to do so will result in the WebSocket connection being closed with an appropriate [disconnect code](disconnect_codes.md).

To promptly detect WebSocket connection issues during idle periods, clients must send a [handshake `keepalive`](handshake/keepalive.md) message at intervals specified in the [handshake `state`](handshake/state.md). The server echoes the received [handshake `keepalive`](handshake/keepalive.md).

### Request

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| request | string | + | [request type](requests/index.md) |
| transaction | string | + | unique transaction identifier of request |
| line | number | +[^1] | line number of _Line_ and _Call_ requests (`null` for guest line) |
| call_id | string | +[^2] | call id of _Call_ request |
| | | | additional request properties if needed |

#### Example

```json
{
  "request": "call",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "number": "1234567890",
  "jsep": {
    "type": "offer",
    "sdp": "..."
  }
}
```

### Response

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| response | string | + | [response type](responses/index.md) |
| transaction | string | + | unique transaction identifier of request |
| line | number | +[^1] | line number of _Line_ and _Call_ requests |
| call_id | string | +[^2] | call id of _Call_ request |
| | | | additional response properties, if needed |

#### Example

```json
{
  "response": "ack",
  "transaction": "transaction-1",
  "line": 0
}
```

### Event

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| event | string | + | [event type](events/index.md) |
| transaction | string | | unique transaction identifier of request issued this event |
| line | number | +[^1] | line number of _Line_ and _Call_ events |
| call_id | string | +[^2] | call id of _Call_ event |
| | | | additional event properties, if needed |

#### Example

```json
{
  "event": "call",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "callee": "0987654321",
  "caller": "1234567890",
  "caller_display_name": "Test User",
  "jsep": {
    "type": "offer",
    "sdp": "..."
  }
}
```

## Logical groups

**WTSP** organizes its communication into logical groups to handle different aspects of the connection lifecycle and operations. These groups include **Session**, **Line**, and **Call**, each serving a distinct purpose.

### Session

A **Session** represents a unique connection between the **WebTrit apps** and the **WebTrit Core**. During the session’s lifecycle, the **WebTrit Core** establishes and manages a corresponding `SIP` registration on the SIP server.

**Sessions** can be classified into two types:
* *permanent*: `SIP` registration persists even without an active session.
* *transient*: `SIP` registration remains active only while the session is active.

By default, all sessions are _transient_. A session becomes _permanent_ if a push token of a specific type is provided for the **WebTrit apps**. For more details on push tokens, refer to the [/app/push-tokens](https://webtrit.github.io/webtrit_core_pages/#/app/createAppPushToken) API.

### Line

A **Line** is a logical container for handling one active call at a time. Lines are identified using the `line` number property in `request`, `response`, and `event` messages.

Key points about lines:
* Lines allow **WebTrit apps** to manage multiple simultaneous calls within a single session.
* The number of available lines for a session is configured on the **WebTrit Core**.
* The current count and state of lines for a particular session can be retrieved from the `lines` property in the [handshake `state`](handshake/state.md).
* The special guest line type exist for making outgoing call with alternate from number. `line` value for such type of line is `null`.

Behavior:
1. _Outgoing Calls:_
To initiate an outgoing call, a free line must be selected. Attempting to place a call on a line with an existing active call will result in an error.
2. _Incoming Calls:_
Incoming calls are automatically assigned to the first available free line. If all lines are occupied with active calls, the new incoming call will be automatically rejected with the appropriate disconnect code.

### Call

A **Call** represents an active outgoing or incoming call and is identified by the `call_id` string property in `request`, `response` and `event` messages.

Key characteristics of a call:
* A call exists within a specific **Line**.

## Features

[Self-recoverable mechanism](features/self_recoverable_mechanism.md)

## Walkthrough

[Usage examples](examples/usage.md)

[^1]: Only for _Line_ and _Call_.
[^2]: Only for _Call_.
