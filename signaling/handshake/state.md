# Handshake state

Handshake `state` sends only ones after a successful Websocket connection opens. Any requests can be sent only after this event is received.

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| handshake | string | + | `state` value |
| keepalive_interval | number | + | signalling keepalive interval in milliseconds |
| timestamp | number | + | server system time in milliseconds |
| registration | [RegistrationObject](#registrationobject) | + | session registration state |
| lines | array of [LineObject](#lineobject) | + | session lines states |

#### RegistrationObject

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| status | [RegistrationStatusString](#registrationstatusstring) | + | registration status |
| code | number | | failed registration code |
| reason | string | | failed registration reason |

#### RegistrationStatusString

Values:
- `registering`
- `registered`
- `registration_failed`
- `unregistering`
- `unregistered`

#### LineObject

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| call_id | string | + | active call id |
| call_logs | array of [CallLog](#calllog) | + | active call id logs |

#### CallLog

| Index | Type | Required | Description |
| --- | --- | :---: | --- |
| 0 | number | + | request or event server system time in milliseconds |
| 1 | object | + | request or event object |

## Example

Three lines with an active call on the first one and idle on others.

```json
{
  "handshake": "state",
  "keepalive_interval": 30000,
  "timestamp": 1662114679648,
  "registration": {
    "status": "registered"
  },
  "lines": [
    {
      "call_id": "qwertyuiopasdfghjklzxcvbnm",
      "call_logs": [
        [
          1662114479751,
          {
            "event": "incoming_call",
            "callee": "123",
            "caller": "456"
          }
        ]
      ]
    },
    null,
    null
  ]
}
```
