# WebTrit Signaling Protocol

**WebTrit Signaling Protocol (WTSP)** goal is to organize reliable, predictable, and self-recoverable mechanisms to connect **WebTrit apps** with **WebTrit Core**.

WTSP uses WebSocket as a communications protocol, with obligatory specifying sub-protocol to [`webtrit-protocol`](websocket_subprotocol.md).

WTSP URL structure:
```
wss://<WebTrit Core host>[:<WebTrit Core port>]/signaling/v1?token=<token>[&force=<true | false (by default)>]
```
where:
- `token` - session token received with [WebTrit Core API](../api/index.md#core)
- `force` - flag to determine, do current connection must close the active one if it exit or not

WTSP message must be string type with JSON serialized data.

## Messages

WTSP uses the next messages types:
- `handshake`
  - *from server to client* and *from client to server*
  - support current connection to an active client session
- `request`
  - *from client to server*
  - logically divided into three groups:
    - _Session_
    - _Line_
    - _Call_
- `response`
  - *from server to client*
  - acknowledge received `request`
- `event`
  - *from server to client*
  - logically divided into three groups:
    - _Session_
    - _Line_
    - _Call_

### Handshake

After a successful WebSocket connection open the **WebTrit Core** server sends the current session [handshake `state`](handshake/state.md). Any requests from client to server can be sent only after this event is received, otherwise, the WebSocket connection will be closed with the appropriate [disconnect code](disconnect_codes.md).

To detect any connection issue in idle connection as soon as possible next approach must be used/implemented. The client must send a [handshake `keepalive`](handshake/keepalive.md) with the interval provided in the [handshake `state`](handshake/state.md). The server echoes the received [handshake `keepalive`](handshake/keepalive.md).

### Request

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| request | string | + | [request type](requests/index.md) |
| transaction | string | + | unique transaction identifier of request |
| line | number | +[^1] | linue numer of _Line_ and _Call_ request |
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
| line | number | +[^1] | linue numer of _Line_ and _Call_ request |
| call_id | string | +[^2] | call id of _Call_ request |
| | | | additional response properties if needed |

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
| line | number | +[^1] | linue numer of _Line_ and _Call_ event |
| call_id | string | +[^2] | call id of _Call_ event |
| | | | additional event properties if needed |

#### Example

```json
{
  "event": "call",
  "line": 0
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

[^1]: Only for _Line_ and _Call_.
[^2]: Only for _Call_.
