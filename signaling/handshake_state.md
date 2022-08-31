# Handshake state

Handshake `state` sends only ones after a successful Websocket connection opens. Any requests can be sent only after this event is received.

| Key | Type | Required | Description |
| --- | --- | --- | --- |
| handshake | string | + | currently awaliable only `state` value |
| keepalive_interval | number | + | signalling keepalive interval in milliseconds |
| timestamp | number | + | server system time in milliseconds |
| registration | object | + | session registration state |
| lines | array of object | + | session lines states |

**Registration object**

| Key | Type | Required | Description |
| --- | --- | --- | --- |
| status | string | + | registration status |
| code | number | | failed registration code |
| reason | string | | failed registration reason |

**Registration status** values:
- registering
- registered
- registration_failed
- unregistering
- unregistered

**Line object**

| Key | Type | Required | Description |
| --- | --- | --- | --- |
| call_id | string | + | active call id |
| call_logs | array of array | + | active call id logs |

**Call log array**

| Index | Type | Required | Description |
| --- | --- | --- | --- |
| 0 | number | + | request or event server system time in milliseconds |
| 1 | object | + | request or event object |

#### Example

```json
{
  "event": "state",
  "keepalive_interval": 30000,
  "timestamp": 1662114679648,
  "registration": {
    "status": "registered"
  },
  "lines": [
    {
      "call_id": "qwertyuiopasdfghjklzxcvbnm",
      "call_logs": [
        [1662114479751, <request or event object>],
        ...
      ]
    },
    null,
    null
  ]
}
```
