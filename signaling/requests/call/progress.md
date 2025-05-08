# `progress` call request

Optionally, progress the incoming call. From `SIP` side this will result in a `183 Session Progress` to be sent back to the caller.

If [`incoming_call`](../../events/call/incoming_call.md) event contained an [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer an `progress` request must always be accompanied by a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer via `jsep` property. An [`progressed`](../../events/call/progressed.md) event (without any properties) will be sent back to confirm the call can be considered established.

In case it was an offer-less `INVITE` and [`incoming_call`](../../events/call/incoming_call.md) event does not contain an [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer an `progress` request must always be accompanied by a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer via `jsep` property. An [`progressing`](../../events/call/progressing.md) event will be sent back after `progress` request start processing. An [`progressed`](../../events/call/progressed.md) event will only follow later, with a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer within `jsep` property (as soon as it is available in the `SIP ACK` the caller sent back).

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| jsep | [RTCSessionDescription](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription) | + | session description |

## Example

```json
{
  "request": "progress",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "jsep": {
    "...": "...",
  }
}
```
