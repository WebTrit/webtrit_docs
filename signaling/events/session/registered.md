# `registered` session event

A successful `SIP` registration notification.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| authorization | [AuthorizationStatusString](#mediatypestring) | | |

#### MediaTypeString

AuthorizationStatusString:
- `ok`
- `failure`

## Example

```json
{
  "event": "registered"
}
```
