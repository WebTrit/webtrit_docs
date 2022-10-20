# `accept` call request

Accept the incoming call. From `SIP` side this will result in a `200 OK` to be sent back to the caller.

If [`incoming_call`](../../events/call/incoming_call.md) event contained an [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer an `accept` request must always be accompanied by a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer via `jsep` property. An [`accepted`](../../events/call/accepted.md) event (without any properties) will be sent back to confirm the call can be considered established.

In case it was an offer-less `INVITE` and [`incoming_call`](../../events/call/incoming_call.md) event does not contain an [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer an `accept` request must always be accompanied by a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer via `jsep` property. An [`accepting`](../../events/call/accepting.md) event will be sent back after `accept` request start processing. An [`accepted`](../../events/call/accepted.md) event will only follow later, with a [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer within `jsep` property (as soon as it is available in the `SIP ACK` the caller sent back).

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| jsep | [RTCSessionDescription](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription) | + | session description |

## Example

```json
{
  "request": "accept",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "jsep": {
    "...": "...",
  }
}
```
