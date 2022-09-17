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
  - *from server to client*
  - contain data to restore client session state
- `request`
  - *from client to server*
  - logically divided into two groups: 
    - Call signaling
    - ICE
- `response`
  - *from server to client*
  - acknowledge received `request`
- `event`
  - *from server to client*
  - logically divided into two groups: 
    - Call signaling
    - ICE

### Handshake

After a successful WebSocket connection open the **WebTrit Core** server sends the current session [handshake `state`](handshake_state.md). Any requests from client to server can be sent only after this event is received, otherwise, the WebSocket connection will be closed with the appropriate [disconnect code](disconnect_codes.md).

### Request

| Key | Type | Required | Description |
| --- | --- | --- | --- |
| request | string | + | [request type](requests/index.md) |
| line | number | + | linue numer of all request |
| transaction | string | + | unique transaction identifier |
| call_id | string | | call id of call signaling request |
| data | object | | request data if necessary |

#### Example

```json
{
  "request": "call",
  "line": 0,
  "transaction": "transaction-1",
  "call_id": "qwertyuiopasdfghjklzxcvbnm",
  "data": {
    "number": "1234567890",
    "jsep": {
      "type": "offer",
      "sdp": "..."
    }
  }
}
```

### Response

| Key | Type | Required | Description |
| --- | --- | --- | --- |
| response | string | + | [response type](responses/index.md) |
| line | number | + | linue numer of all request |
| transaction | string | + | unique transaction identifier of request |
| data | object | | response data if necessary |

#### Example

```json
{
  "response": "ack",
  "line": 0,
  "transaction": "transaction-1"
}
```

### Event

| Key | Type | Required | Description |
| --- | --- | --- | --- |
| event | string | + | [event type](events/index.md) |
| line | number | + | linue numer of event |
| transaction | string | | unique transaction identifier of request issued this event |
| call_id | string | | call id of call signaling event |
| data | object | | event data if necessary |

#### Example

```json
{
  "event": "call",
  "line": 0
  "call_id": "qwertyuiopasdfghjklzxcvbnm",
  "data": {
    "callee": "0987654321",
    "caller": "1234567890",
    "caller_display_name": "Test User",
    "jsep": {
      "type": "offer",
      "sdp": "..."
    }
  }
}
```

## Keepalive

To detect any connection issue as soon as possible, the client must send [`keepalive` request](requests/keepalive.md) with the interval provided in [handshake `state`](handshake_state.md).
