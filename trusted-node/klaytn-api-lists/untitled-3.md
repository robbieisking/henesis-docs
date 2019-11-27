---
description: Returns the number of most recent block.
---

# klay\_blockNumber

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_blockNumber","params":[],"id":83}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 83,
    "result": "0xb2e441"
}
```

#### Parameters

none

#### Returns

`QUANTITY`: Integer of the current block number the client is on.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_blocknumber](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_blocknumber)



