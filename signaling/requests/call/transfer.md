# `transfer` call request

Attended or blind transfer the call. A [`transferring`](../../events/call/transferring.md) event will be sent back after `transfer` request start processing.

## Additional request properties

| Key | Type | Required | Description |
| --- | --- | :---: | --- |
| number | string | + | number to transfer the call |
| replace_call_id | string | | Call-ID of the call this attended transfer is supposed to replace; default is none, which means blind/unattended transfer |
