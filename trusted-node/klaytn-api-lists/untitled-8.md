---
description: >-
  Returns the number of transactions in a block from a block matching the given
  block hash.
---

# klay\_getBlockTransactionCountByHash

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getBlockTransactionCountByHash","params":["0x348f0b61223c046b395d8f4c99be394384cb8034d83c6de55890ea5aa62ddc31"],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x1"
}
```

#### Parameters

* `32-byte DATA`: Hash of a block.

**Returns**

`QUANTITY`: Integer of the number of transactions in this block.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getblocktransactioncountbyhash](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getblocktransactioncountbyhash)

