---
description: >-
  Returns information about a block by hash. This API works only on RPC call,
  not on Javascript console.
---

# klay\_getBlockByHash

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getBlockByHash","params":["0xb8deae63002d2b6aa33247c8ef545383ee0fd2282ac9b49dbbb74114389ddb5c", true],"id":1}'
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

* `32-byte DATA`: Hash of a block.
* `Boolean` : If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

#### Returns

`Object` - A block object, or `null` when no block was found:

| Name | Type | Description |
| :--- | :--- | :--- |
| number | QUANTITY | The block number. `null` when it is pending block. |
| hash | 32-byte DATA | Hash of the block. `null` when it is pending block. |
| parentHash | 32-byte DATA | Hash of the parent block. |
| logsBloom | 256-byte DATA | The bloom filter for the logs of the block. `null` when it is pending block. |
| transactionsRoot | 32-byte DATA | The root of the transaction trie of the block. |
| stateRoot | 32-byte DATA | The root of the final state trie of the block. |
| receiptsRoot | 32-byte DATA | The root of the receipts trie of the block. |
| reward | 20-byte DATA | The address of the beneficiary to whom the block rewards were given. |
| blockScore | QUANTITY | Former difficulty. Always 1 in the BFT consensus engine |
| totalBlockScore | QUANTITY | Integer of the total blockScore of the chain until this block. |
| extraData | DATA | The "extra data" field of this block. |
| size | QUANTITY | Integer the size of this block in bytes. |
| gasUsed | QUANTITY | The total used gas by all transactions in this block. |
| timestamp | QUANTITY | The Unix timestamp for when the block was collated. |
| timestampFoS | QUANTITY | The fraction of a second of the timestamp for when the block was collated. |
| transactions | Array | Array of transaction objects, or 32-byte transaction hashes depending on the last given parameter. |
| governanceData | DATA | RLP encoded governance configuration |
| voteData | DATA | RLP encoded governance vote of the proposer |

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getblockbyhash](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getblockbyhash)

