---
description: Returns the number of transactions in a block matching the given block number.
---

# eth\_getBlockTransactionCountByNumber

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa" // 10
}
```

#### Parameters

* `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter).

#### Returns

`QUANTITY` - integer of the number of transactions in this block.

#### Reference

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getblocktransactioncountbynumber](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getblocktransactioncountbynumber)

