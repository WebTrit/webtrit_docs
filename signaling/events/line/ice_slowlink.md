# `ice_slowlink` line event

Notifying that the WebRTC server is reporting trouble sending/receiving (`uplink: true/false`) media on this `PeerConnection`.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| type | [MediaTypeString](#mediatypestring) | + | |
| uplink | boolean | + | |
| lost | number | + | number of lost packets are detected within a second |

#### MediaTypeString

Values:
- `audio`
- `video`

## Example

```json
{
  "event": "ice_slowlink",
  "line": 0,

  "type": "audio",
  "uplink": true,
  "lost": 10
}
```
