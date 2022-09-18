# Handshake keepalive

Handshake `keepalive` is a unique message that sends *from client to server* and *from server to client*. But only the client can be the initiator of handshake `keepalive` exchange. The server only echoes the received handshake `keepalive`` back to the client.

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| handshake | string | + | `keepalive` value |
| ... | | | |

## Example

Simplest handshake `keepalive` message:

```json
{
  "handshake": "keepalive"
}
```

More reasonable handshake `keepalive` message:

```json
{
  "handshake": "keepalive",
  "timestamp": 1663526263768
}
```
