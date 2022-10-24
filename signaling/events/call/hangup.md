# `hangup` call event

Notifying that the call is declined or being hung up. Which is basically a `SIP` error event notification as it includes the `code` and `reason` properties. A regular `BYE`, for instance, would be notified with `200` and `SIP BYE`, although a more verbose description may be provided as well.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| code | number | + | |
| reason | number | + | |

## Example

```json
{
  "event": "hangup",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "code": 200,
  "reason": "Session Terminated"
}
```
