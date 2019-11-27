---
description: >-
  Returns information about a transaction by block number and transaction index
  position. This API works only on RPC call, not on Javascript console.
---

# klay\_getTransactionByBlockNumberAndIndex

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getTransactionByBlockNumberAndIndex","params":["0x27", "0x0"],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockHash": "0xfe765aa12625149df4d420a62e11e8609a7969c8eff046d6ffabd4163b9e994a",
        "blockNumber": "0xb2f2ca",
        "feePayer": "0xea6a33ae8f23de7a523c9edc5da45391bf167757",
        "feePayerSignatures": [
            {
                "V": "0x4056",
                "R": "0xae0ab667598505bab9fbd9da281605eded5ae3d7efd054e9e336f7e0f8b1ced4",
                "S": "0x1b9261acebaa46d00bd9e1a801a9005629a363853ee4d19921dda2357716b12"
            }
        ],
        "from": "0x35af410303ec81579bc8360b0d0aa8e3d27eaf51",
        "gas": "0x15f90",
        "gasPrice": "0x5d21dba00",
        "hash": "0x86ab2317c3ea04886566993be1d04b2f59c9e1abd88925b7ecc88a002a856d12",
        "input": "0xa9059cbb000000000000000000000000ac97d56505d0ff5889175608a8ab14221047c15200000000000000000000000000000000000000000000065a4da25d3016c00000",
        "nonce": "0x6",
        "senderTxHash": "0x22e4fae7bd4e925499c6d59dff83dee3f5c60b6988d0787c770dd667ab3c2078",
        "signatures": [
            {
                "V": "0x4056",
                "R": "0x2d397c9731c35210f57c53143a4eec24cc07db7af7072ae7f338c9842639f397",
                "S": "0x34c6ab2eac07fdfc39a44449a2eac8c9cb3f40e57b0aa78dfe4cf5e4d2a37e52"
            }
        ],
        "to": "0xbe7377db700664331beb28023cfbd46de079efac",
        "transactionIndex": "0x0",
        "type": "TxTypeFeeDelegatedSmartContractExecution",
        "typeInt": 49,
        "value": "0x0"
    }
}
```

#### Parameters

* `20-byte DATA`: Address to check for balance.
* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`Object` - A transaction object, or `null` when no transaction was found:

| Name | Type | Description |
| :--- | :--- | :--- |
| blockHash | 32-byte DATA | Hash of the block where this transaction was in. `null` when it is pending. |
| blockNumber | QUANTITY | Block number where this transaction was in. `null` when it is pending. |
| codeFormat | String | \(optional\) The code format of smart contract code. |
| feePayer | 20-byte DATA | \(optional\) Address of the fee payer. |
| feePayerSignatures | Array | \(optional\) An array of fee payer's signature objects. A signature object contains three fields \(V, R, and S\). V contains ECDSA recovery id. R contains ECDSA signature r while S contains ECDSA signature s. |
| feeRatio | QUANTITY | \(optional\) Fee ratio of the fee payer. If it is 30, 30% of the fee will be paid by the fee payer. 70% will be paid by the sender. |
| from | 20-byte DATA | Address of the sender. |
| gas | QUANTITY | Gas provided by the sender. |
| gasPrice | QUANTITY | Gas price provided by the sender in peb. |
| hash | 32-byte DATA | Hash of the transaction. |
| humanReadable | Boolean | \(optional\) `true` if the address is humanReadable, `false` if the address is not humanReadable. |
| key | String | \(optional\) Key of the newly created account. |
| input | DATA | \(optional\) The data sent along with the transaction. |
| nonce | QUANTITY | The number of transactions made by the sender prior to this one. |
| senderTxHash | 32-byte DATA | Hash of a transaction that is signed only by the sender. See [SenderTxHash](). This value is always the same as `hash` for non fee-delegated transactions. |
| signatures | Array | An array of signature objects. A signature object contains three fields \(V, R, and S\). V contains ECDSA recovery id. R contains ECDSA signature r while S contains ECDSA signature s. |
| to | 20-byte DATA | Address of the receiver. `null` when it is a contract creation transaction. |
| transactionIndex | QUANTITY | Integer of the transaction index position in the block. `null` when it is pending. |
| type | String | A string representing the type of the transaction. |
| typeInt | QUANTITY | An integer representing the type of the transaction. |
| value | QUANTITY | Value transferred in peb. |

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction\#klay\_gettransactionbyblocknumberandindex](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction#klay_gettransactionbyblocknumberandindex)

