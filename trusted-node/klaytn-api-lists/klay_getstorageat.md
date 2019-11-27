---
description: Returns the value from a storage position at a given address.
---

# klay\_getStorageAt

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0", "method": "klay_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

#### Parameters

* `20-byte DATA`: Address of the storage.
* `QUANTITY`: Integer of the position in the storage.
* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`DATA` : The value at this storage position.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getstorageat](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getstorageat)

