# `ack` response

Acknowledge that the corresponding request has been received, checked, and started processing.

## Additional response properties

None.

## Example

```json
{
  "response": "ack",
  "transaction": "transaction-1",
  "line": 0
}
```

```json
{
  "response": "ack",
  "transaction": "transaction-2",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```
