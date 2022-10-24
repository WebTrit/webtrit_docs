# `incoming_call` call event

Notifying about incomming call, that is actullay `SIP INVITE`.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| callee | string | + | |
| caller | string | + | |
| caller_display_name | string | | display name of the caller |
| replace_call_id | string | | Call-ID of the call that this is supposed to replace, if this is an attended transfer |
| jsep | [RTCSessionDescription](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription) | | session description |

## Example

```json
{
  "event": "incoming_call",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "callee": "123",
  "caller": "456",
  "caller_display_name": "Some Name",
  "jsep": {
    "type": "offer",
    "sdp": "..."
  }
}
```
