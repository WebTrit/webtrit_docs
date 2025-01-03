# `accepted` call event

An [`accept`](../../requests/call/accept.md) request with [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) answer starts processing notification. In this case no `jsep` propery provided.

An [`accept`](../../requests/call/accept.md) request with [SDP](https://developer.mozilla.org/en-US/docs/Glossary/SDP) offer finish processing notification. In this case `jsep` propery provided.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| is_focus | boolean | | Indicate is `isfocus` feature parameter in the Contact header |
| jsep | [RTCSessionDescription](https://developer.mozilla.org/en-US/docs/Web/API/RTCSessionDescription) | | session description |

## Example

```json
{
  "event": "accepted",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```

```json
{
  "event": "accepted",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "jsep": {
    "type": "answer",
    "sdp": "..."
  }
}
```
