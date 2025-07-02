# `notify` call event

Notifying about incomming notify, that is actullay `SIP NOTIFY`.

## Base notify event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| notify | string | | name of the event that the user is subscribed to |
| subscription_state | [SubscriptionStateString](#subscriptionstatestring) | | |

## Raw notify event sub-properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| content_type | string | | content-type of the message |
| content | string | + | content of the message |

## Dialogs notify event sub-properties
| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| user_active_calls | array of [UserActiveCall](#useractivecall) | + | user active calls |

#### SubscriptionStateString

Values:
- `pending`
- `terminated`
- `active`

#### UserActiveCall
| Index | Type | Required | Description |
| --- | --- | :---: | --- |
| id | string | + | id |
| state | string | + | call state e.g "proceeding \| confirmed" |
| call_id | string | + | call id |
| direction | string | + | call direction e.g "initiator \| recipient" |
| local_tag | string | + | local tag |
| remote_tag | string | + | remote tag |
| remote_number | string | + | calle sip number |
| remote_display_name | string | + | calle display name |

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


{
  "event": "notify",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "notify": "dialog",
  "subscription_state": "active",
  "user_active_calls": [
    {
      "id": "asldkaslkdkasdj",
      "state": "confirmed",
      "call_id": "qwertyuiopasdfghjklzxcvbnm",
      "direction": "initiator",
      "local_tag": "local-tag-123",
      "remote_tag": "remote-tag-456",
      "remote_number": "+1234567890",
      "remote_display_name": "John Doe"
    }
  ]
}

```
