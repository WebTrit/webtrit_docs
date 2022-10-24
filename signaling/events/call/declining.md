# `declining` call event

A [`decline`](../../requests/call/decline.md) request starts processing notification.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| code | number | + | responce code |
| refer_id | number | | if there're declining a call transfer, rather than an incoming call |

## Example

```json
{
  "event": "declining",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "code": 486
}
```
