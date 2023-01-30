# `missed_call` call event

Notifying that there are no empty lines to process the incomming call. In this case incomming call ended with `486` code.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| callee | string | + | |
| caller | string | + | |
| caller_display_name | string | | display name of the caller |

## Example

```json
{
  "event": "missed_call",
  "line": 0,
  "call_id": "qwertyuiopasdfghjklzxcvbnm",

  "callee": "123",
  "caller": "456",
  "caller_display_name": "Some Name"
}
```
