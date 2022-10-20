# `hangup` call request

Hang up the active call. An [`hangingup`](../../events/call/hangingup.md) event will be sent back after `hangup` request start processing. An [`hangup`](../../events/call/hangup.md) event will be sent back to confirm the call is hung up.

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
