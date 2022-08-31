# Disconnect codes

In case when server trap in any unreliable state it closes the WebSocket connection with the appropriate disconnect code and reason.

| Code | Description |
| ---- | --- |
| 4101 | socket message error |
| 4201 | session missed error |
| 4300 | app unknown error |
| 4301 | app missed error |
| 4302 | app unregistered error |
| 4400 | controller unknown error |
| 4401 | controller missed error |
| 4402 | controller exit error |
| 4410 | controller subscriber data adapter error |
| 4411 | controller subscriber missed error |
| 4412 | controller subscriber credentials missed error |
| 4420 | controller webrtc error |
| 4431 | controller attached error |
| 4432 | controller not attached error |
| 4441 | controller force attach close |
| 4501 | signaling message error |
| 4502 | signaling keepalive timeout error |
| 4600 | request unknown error |
| 4601 | request execute error |
| 4602 | request execute timeout error |
| 4610 | request call id error |
