# Event error codes

| Code | Description |
| ---- | --- |
| 499 | Unknown error |
| 440 | No message |
| 441 | Invalid JSON |
| 442 | Invalid request |
| 443 | Missing element |
| 444 | Invalid element |
| 445 | Already registered |
| 446 | Invalid address |
| 447 | Wrong state |
| 448 | Missing SDP |
| 449 | Libsofia error |
| 450 | IO error |
| 451 | Recording error |
| 452 | Too strict |
| 453 | Helper error |
| 454 | No such Call-ID |

This table lists *virtual/internal error codes* specific to Janus, as listed in the [Janus SIP plugin source code](https://github.com/meetecho/janus-gateway/blob/4419baf07a8dfe462346bbb67a51da6504596f84/src/plugins/janus_sip.c#L1639).
Any error codes not included in this table should be interpreted as standard [SIP response codes](https://en.wikipedia.org/wiki/List_of_SIP_response_codes).
