# `notify` call event

Notifying about incomming notify, that is actullay `SIP NOTIFY`.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| notify | string | | name of the event that the user is subscribed to |
| subscription_state | [SubscriptionStateString](#subscriptionstatestring) | | |
| content_type | string | | content-type of the message |
| content | string | + | content of the message |

#### SubscriptionStateString

Values:
- `pending`
- `terminated`
- `active`

## Example

```json
{
  "event": "notify",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "notify": "refer",
  "subscription_state": "active",
  "content_type": "message/sipfrag",
  "content": "SIP/2.0 100 Trying\r\n"
}
```
