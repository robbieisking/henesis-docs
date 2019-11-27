---
description: >-
  Returns information about a transaction by block hash and transaction index
  position.
---

# eth\_getTransactionByBlockHashAndIndex

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xba15f44bef450a975c0e7938e8984366388ec07b01bb7c37204a2a1231ee4332", "0x0"],"id":1}'
```

#### Response

```javascript
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber":"0x5daf3b", // 6139707
    "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas":"0xc350", // 50000
    "gasPrice":"0x4a817c800", // 20000000000
    "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input":"0x68656c6c6f21",
    "nonce":"0x15", // 21
    "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex":"0x41", // 65
    "value":"0xf3dbb76162000", // 4290000000000000
    "v":"0x25", // 37
    "r":"0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
    "s":"0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
  }
}
```

#### Parameters

* `DATA`, 32 Bytes - hash of a block.
* `QUANTITY` - integer of the transaction index position.

#### Returns <a id="returns"></a>

`Object` - A transaction object, or `null` when no transaction was found:

| Name | Type | Description |
| :--- | :--- | :--- |
| blockHash | 32-byte DATA | Hash of the block where this transaction was in. `null` when it is pending. |
| blockNumber | QUANTITY | Block number where this transaction was in. `null` when it is pending. |
| from | 20-byte DATA | Address of the sender. |
| gas | QUANTITY | Gas provided by the sender. |
| gasPrice | QUANTITY | Gas price provided by the sender in peb. |
| hash | 32-byte DATA | Hash of the transaction. |
| humanReadable | Boolean | \(optional\) `true` if the address is humanReadable, `false` if the address is not humanReadable. |
| key | String | \(optional\) Key of the newly created account. |
| input | DATA | \(optional\) The data sent along with the transaction. |
| nonce | QUANTITY | The number of transactions made by the sender prior to this one. |
| to | 20-byte DATA | Address of the receiver. `null` when it is a contract creation transaction. |
| transactionIndex | QUANTITY | Integer of the transaction index position in the block. `null` when it is pending. |
| value | QUANTITY | Value transfered in Wei. |
| v | QUANTITY | ECDSA recovery id |
| r | QUANTITY | ECDSA recovery r |
| s | QUANTITY |  ECDSA recovery s |

**Reference**

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_gettransactionbyblockhashandindex](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactionbyblockhashandindex)​[  
](https://docs.tn.henesis.io/ethereum/eth_getstorageat)

