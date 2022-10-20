# `unhold` call request

Resume the call from on-hold. A [`resuming`](../../events/call/resuming.md) event will be sent back after `unhold` request start processing.

## Additional request properties

None.

## Example

```json
{
  "request": "unhold",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```
