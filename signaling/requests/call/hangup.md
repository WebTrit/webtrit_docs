# `hangup` call request

Hang up the active call. A [`hangingup`](../../events/call/hangingup.md) event will be sent back after `hangup` request start processing. A [`hangup`](../../events/call/hangup.md) event will be sent back to confirm the call is hung up.

## Additional request properties

None.

## Example

```json
{
  "request": "hangup",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```
