# `proceeding` call event

Notifying that the outgoing call is in the proceeding state, so the client can play a ringback tone for the user since there are no early media sent.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| code | number | + | |

## Example

```json
{
  "event": "missed_call",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "code": 180
}
```
