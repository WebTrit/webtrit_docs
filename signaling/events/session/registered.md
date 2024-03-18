# `registered` session event

A successful `SIP` registration notification.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| phrase | string | | a message that is additionally received together with the confirmation of registration |

#### Typical `phrase`:
- `ok`
- `authorization_failure`

## Example

```json
{
  "event": "registered",
  "phrase": "ok"
}
```
