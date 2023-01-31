# `error` line event

Notifying that the line error occurs.

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

  "code": 440,
  "reason": "No message??"
}
```
