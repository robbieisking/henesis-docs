---
description: Returns the number of transactions in a block matching the given block number.
---

# klay\_getBlockTransactionCountByNumber

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x0"
}
```

#### Parameters

* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`QUANTITY`: Integer of the number of transactions in this block.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getblocktransactioncountbynumber](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getblocktransactioncountbynumber)

