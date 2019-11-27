---
description: >-
  Returns the number of uncles in a block from a block matching the given block
  number.
---

# eth\_getUncleCountByBlockNumber

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getUncleCountByBlockNumber","params":["0xe8"],"id":1}'
```

#### Response

```javascript
// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

#### Parameters

* `QUANTITY|TAG` - integer of a block number, or the string "latest", "earliest" or "pending", see the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter).

#### Returns

`QUANTITY` - integer of the number of uncles in this block.

**Reference**

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getunclecountbyblocknumber](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getunclecountbyblocknumber)

