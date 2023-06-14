# `transferring` call event

Notifying that the transfer is in the transferring state. Further updates will come in the form of [`notify`](./notify.md) events.

## Additional event properties

None.

## Example

```json
{
  "event": "transferring",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```
