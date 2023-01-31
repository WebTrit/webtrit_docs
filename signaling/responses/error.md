# `error` response

Inform that the corresponding request has been received, but a specified error occurs on checking or starting processing.

## Additional response properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| code | number | + | [response error codes](error_codes.md) |
| reason | string | + | |

## Example

```json
{
  "response": "error",
  "transaction": "transaction-13",
  "line": 0,

  "code": 472,
  "reason": "Currently not accepting new sessions"
}
```
