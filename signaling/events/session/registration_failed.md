# `registration_failed` session event

A failed `SIP` registration notification.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| code | number | + | |
| reason | string | + | |

## Example

```json
{
  "event": "registration_failed",

  "code": 403,
  "reason": "Forbidden"
}
```
