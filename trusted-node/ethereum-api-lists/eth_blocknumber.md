---
description: Returns the number of most recent block.
---

# eth\_blockNumber

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

#### Response

```javascript
{
  "id":83,
  "jsonrpc": "2.0",
  "result": "0xc94" // 1207
}
```

#### Parameters

none

#### Returns

`QUANTITY` - integer of the current block number the client is on.

**Reference**

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_blocknumber](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_blocknumber)

