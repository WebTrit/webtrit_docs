# `ice_media` line event

Notifying that the WebRTC server is receiving audio/video on this `PeerConnection`.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| type | [MediaTypeString](#typestring) | + | |
| receiving | boolean | + | |

#### MediaTypeString

Values:
- `audio`
- `video`

## Example

```json
{
  "event": "ice_media",
  "line": 0,

  "type": "audio",
  "receiving": true
}
```
