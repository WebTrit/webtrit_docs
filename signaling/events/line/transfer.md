# `transfer` line event

Notifying recipient about transfer request, that is actullay `REFER`.

## Additional event properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| refer_id | number | + | |
| refer_to | string | + | |
| referred_by | string | | number of lost packets are detected within a second |
| replace_call_id | string | | Call-ID of the call that this is supposed to replace, if this is an attended transfer |

## Example

```json
{
  "event": "transfer",
  "line": 0,

  "refer_id": 123,
  "refer_to": "007",
  "referred_by": "001"
}
```
