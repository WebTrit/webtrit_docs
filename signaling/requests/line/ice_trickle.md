# `ice_trickle` line request

Transmit the candidate to the remote peer.

More info:
* [RFC 8838: Trickle ICE: Incremental Provisioning of Candidates for the Interactive Connectivity Establishment (ICE) Protocol](https://www.rfc-editor.org/rfc/rfc8838.html)

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| candidate | [RTCIceCandidate](https://developer.mozilla.org/en-US/docs/Web/API/RTCIceCandidate) | + | the ICE candidate that has been identified and added to the local peer, or `null` to indicate that there are no further candidates for this negotiation session |

## Example

A single trickle candidate message:

```json
{
  "request": "ice_trickle",
  "transaction": "transaction-1",
  "line": 0,

  "candidate": {
    "candidate": "candidate:430735571 1 udp 2122260223 192.168.1.100 53252 typ host generation 0 ufrag eZoF network-id 1 network-cost 10",
    "sdpMLineIndex": 0,
    "sdpMid": "audio"
  }
}
```

A `null` candidate or a `completed` JSON object to notify the end of the candidates messages:

```json
{
  "request": "ice_trickle",
  "transaction": "transaction-2",
  "line": 0,

  "candidate": null
}
```

```json
{
  "request": "ice_trickle",
  "transaction": "transaction-2",
  "line": 0,

  "candidate": {
    "completed": true
  }
}
```
