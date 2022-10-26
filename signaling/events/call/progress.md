# `progress` call event

Notifying that the outgoing call is in the proceeding state and has early media, so the client can fihish WebRTC negotiation with [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer in `jsep` property in this event, before call answered.

In case the caller received a `progress` event, the following [`accepted`](./accepted.md) event will not contain a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer in `jsep` property.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| callee | string | + | |
| jsep | [RTCSessionDescription](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription) | + | session description |

## Example

```json
{
  "event": "progress",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "callee": "123",
  "jsep": {
    "type": "answer",
    "sdp": "..."
  }
}
```
