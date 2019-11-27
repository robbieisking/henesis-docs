---
description: >-
  Returns the number of transactions in a block from a block matching the given
  block hash.
---

# eth\_getBlockTransactionCountByHash

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0x375c6be8ee2c0e6d9c9bd64aea4b0da724eb9883ef45a96e7bb1f90d50a3cc82"],"id":1}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xc" // 11
}
```

#### Parameters

* `DATA`, 32 Bytes - hash of a block.

#### Returns

`QUANTITY` - integer of the number of transactions in this block.

#### Reference

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getblocktransactioncountbyhash](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getblocktransactioncountbyhash)

