# `keyframe` call request

To programmatically send a video keyframe request to either the WebRTC user or the SIP peer (or both), the `keyframe` request can be used. This request is particularly useful when the SIP peer doesn't support RTCP PLI, and so may use other mechanisms (e.g., via signalling) to ask for a keyframe to get video working. By using this request, the WebRTC user can ask Janus to originate a PLI programmatically. The direction of the keyframe request can be provided by using the `user` and `peer` properties: if `user` is `true` a keyframe request will be sent by Janus to the WebRTC user; if `peer` is `true` a keyframe request will be sent by Janus to the SIP peer. In both cases an RTCP PLI message will be sent.
A [`keyframe_sent`](../../events/call/keyframe_sent.md) event will be sent back in case `keyframe` request is successful.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| user | boolean | | whether or not to send a keyframe request to the WebRTC user |
| peer | boolean | | whether or not to send a keyframe request to the WebRTC user |

## Example

```json
{
  "request": "keyframe",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "direction": "inactive"
}
```
