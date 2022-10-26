# `decline` call request

Decline the incoming call that you don't want to [`accept`](./accept.md). In all other cases, use the [`hangup`](./hangup.md) request instead. An [`declining`](../../events/call/declining.md) event will be sent back after `decline` request start processing. An [`hangup`](../../events/call/hangup.md) event will be sent back to confirm the call is declined.

## Additional request properties

None.

## Example

```json
{
  "request": "decline",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```
