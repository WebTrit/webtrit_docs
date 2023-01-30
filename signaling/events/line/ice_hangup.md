# `ice_hangup` line event

Notifying that the `PeerConnection` was closed, either by the WebRTC server or by the user/application, and as such cannot be used anymore.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| reason | string | | |

## Example

```json
{
  "event": "ice_hangup",
  "line": 0,

  "reason": "DTLS alert"
}
```
