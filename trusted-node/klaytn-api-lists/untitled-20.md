---
description: Returns an array of all logs matching a given filter object.
---

# klay\_getLogs

#### Request

```bash
curl https://tn.henesis.io/klaytn/baobab?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getLogs","params":[{"fromBlock":"earliest","toBlock":"latest","address":"0xC1C88A7B90062866F54320b18Ee5DA65F79202D6"}],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "address": "0xc1c88a7b90062866f54320b18ee5da65f79202d6",
            "topics": [
                "0xa9c8dfcda5664a5a124c713e386da27de87432d5b668e79458501eb296389ba7"
            ],
            "data": "0x000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000186a000000000000000000000000000000000000000000000000000000000000186a10000000000000000000000000000000000000000000000000000000000000064",
            "blockNumber": "0x62c699",
            "transactionHash": "0x01788b0c53e1083f7b99cd4780732379bfd4904e7ef782224dfc54288cfd874c",
            "transactionIndex": "0x0",
            "blockHash": "0x99c378c272ff1ac0697655bb32a35300e685c2dc75384a9d0ec8cace2084aac0",
            "logIndex": "0x1",
            "removed": false
        },
        ...
    ]
}
```

#### Parameters

* `Object` - The filter options
  * fromBlock\(`QUANTITY | TAG`\): \(optional, default: "latest"\) Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.
  * toBlock\(`QUANTITY | TAG`\): \(optional, default: "latest"\) Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.
  * address\(`20-byte DATA | Array`\): \(optional\) Contract address or a list of addresses from which logs should originate.
  * topics\(`Array of DATA`\): \(optional\) Array of 32-byte DATA topics. Topics are order-dependent. Each topic can also be an array of DATA with “or” options.
  * blockHash\(`32-byte DATA`\): \(optional\) A filter option that restricts the logs returned to the single block with the 32-byte hash blockHash. Using blockHash is equivalent to fromBlock = toBlock = the block number with hash blockHash. If blockHash is present in in the filter criteria, then neither fromBlock nor toBlock are allowed.

**Returns**

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

\*\*\*\*

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/filter\#klay\_getlogs](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/filter#klay_getlogs)

