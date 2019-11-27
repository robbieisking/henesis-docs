---
description: Returns the value from a storage position at a given address.
---

# eth\_getStorageAt

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}'
```

#### Response

```bash
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0x000000000000000000000000000000000000000000000000000000000000162e"
}
```

#### Parameters

* `DATA`, 20 Bytes - address of the storage.
* `QUANTITY` - integer of the position in the storage.
* `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter)​

#### Returns <a id="returns"></a>

`DATA` - the value at this storage position.

**Reference**

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getstorageat](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getstorageat)​[  
](https://docs.tn.henesis.io/ethereum/eth_getlogs)

