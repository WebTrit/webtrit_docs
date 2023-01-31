# `error` call event

Notifying that the call error occurs.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| code | number | + | [event error codes](../error_codes.md) |
| reason | string | + | |

## Example

```json
{
  "event": "error",
  "transaction": "transaction-13",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "code": 450,
  "reason": "Could not allocate RTP/RTCP ports"
}
```
