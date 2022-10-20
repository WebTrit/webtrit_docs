# `hold` call request

Put the call on-hold. This request will send a new INVITE to the peer. No `WebRTC` renegotiation will be involved on the holder side, as this will only trigger a `re-INVITE` on the `SIP` side. To remove the call from on-hold, send a [`unhold`](./unhold.md) request. A [`holding`](../../events/call/holding.md) event will be sent back after `hold` request start processing.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| direction | [DirectionString](#directionstring) | | hold media direction |

#### DirectionString

Values:
- `sendonly` - default
- `recvonly`
- `inactive`

## Example

```json
{
  "request": "hold",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "direction": "inactive"
}
```
