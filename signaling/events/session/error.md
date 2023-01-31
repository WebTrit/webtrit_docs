# `error` session event

Notifying that the session error occurs.

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

  "code": 499,
  "reason": "Missing session or Sofia stack"
}
```
