# `update` call request

Update an existing session (e.g., to do a renegotiation with add/remove streams or force an `ICE` restart). The whole context is derived from the current state of the session, so it does need only the new [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer via `jsep` property to provide, though, as part of the renegotiation. An [`updating`](../../events/call/updating.md) event will be sent back after `update` request start processing.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| jsep | [RTCSessionDescription](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription) | + | updated session description |

## Example

```json
{
  "request": "update",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "jsep": {
    "...": "...",
  }
}
```
