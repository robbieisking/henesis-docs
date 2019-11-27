---
description: Returns an array of all logs matching a given filter object.
---

# eth\_getLogs

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getLogs","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}],"id":74}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc":"2.0",
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

#### Parameters

* `Object` - The filter options:
  * `fromBlock`: `QUANTITY|TAG` - \(optional, default: `"latest"`\) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  * `toBlock`: `QUANTITY|TAG` - \(optional, default: `"latest"`\) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
  * `address`: `DATA|Array`, 20 Bytes - \(optional\) Contract address or a list of addresses from which logs should originate.
  * `topics`: `Array of DATA`, - \(optional\) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options.
  * `blockhash`: `DATA`, 32 Bytes - \(optional\) With the addition of EIP-234 \(Geth &gt;= v1.8.13 or Parity &gt;= v2.1.0\), `blockHash` is a new filter option which restricts the logs returned to the single block with the 32-byte hash `blockHash`. Using `blockHash` is equivalent to `fromBlock` = `toBlock` = the block number with hash `blockHash`. If `blockHash` is present in the filter criteria, then neither `fromBlock` nor `toBlock` are allowed.

#### Returns <a id="returns"></a>

`Array` - Array of log objects, or an empty array if nothing has changed since last poll.

| Name | Type | Description |
| :--- | :--- | :--- |
| removed | TAG | `true` when the log was removed, due to a chain reorganization. `false` if it is a valid log. |
| logIndex | QUANTITY | Integer of the log index position in the block. `null` when it is a pending log. |
| transactionIndex | QUANTITY | Integer of the transactions index position log was created from. `null` when pending. |
| transactionHash | 32-byte DATA | Hash of the transactions this log was created from. `null` when pending. |
| blockHash | 32-byte DATA | Hash of the block where this log was in. `null` when pending. |
| blockNumber | QUANTITY | The block number where this log was in. `null` when pending. |
| address | 20-byte DATA | Address from which this log originated. |
| data | DATA | Contains the non-indexed arguments of the log. |
| topics | Array of DATA | Array of 0 to 4 32-byte DATA of indexed log arguments. \(In Solidity: The first topic is the hash of the signature of the event \(_e.g._, `Deposit(address,bytes32,uint256)`\), except you declared the event with the `anonymous` specifier.\). |

**Reference**

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getlogs](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getlogs)​[  
](https://docs.tn.henesis.io/ethereum/eth_getcode)

