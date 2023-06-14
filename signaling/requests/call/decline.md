# `decline` call request

Decline the [`incoming_call`](../../events/call/incoming_call.md) that you don't want to [`accept`](./accept.md) or [`transfer`](../../events/line/transfer.md) that you don't want to [`outgoing_call`](outgoing_call.md). In all other cases, use the [`hangup`](./hangup.md) request instead. An [`declining`](../../events/call/declining.md) event will be sent back after `decline` request start processing. An [`hangup`](../../events/call/hangup.md) event will be sent back to confirm the call is declined.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| refer_id | number | | in case this is the result of a [`transfer`](../../events/line/transfer.md) event, the unique identifier that addresses it |

## Example

```json
{
  "request": "decline",
  "transaction": "transaction-1",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm"
}
```
