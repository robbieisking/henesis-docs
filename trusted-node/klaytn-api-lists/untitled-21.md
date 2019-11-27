---
description: >-
  Returns the information about a transaction requested by transaction hash.
  This API works only on RPC call, not on Javascript console.
---

# klay\_getTransactionByHash

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getTransactionByHash","params":["0x716aa19341369daa04bc15248cc274296cc8dac07b2b3225b1f2b75522587ede"],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockHash": "0x348f0b61223c046b395d8f4c99be394384cb8034d83c6de55890ea5aa62ddc31",
        "blockNumber": "0xb2e6ff",
        "feePayer": "0xad7c07eab1e56fbbb976fd5377e5088ec5528cd9",
        "feePayerSignatures": [
            {
                "V": "0x4056",
                "R": "0x6e0a88b0c8d7455d85e19006ff3faf4239e27e33c3cb363dd00456ccbf37dad2",
                "S": "0x5e33e332f4264d31c10a934bc17ba35f1f833176189608c1c9c3ab5edda7018d"
            }
        ],
        "from": "0xecf2d7047dc7634557840cd205f2c6853d022e88",
        "gas": "0x2dc6c0",
        "gasPrice": "0x5d21dba00",
        "hash": "0x716aa19341369daa04bc15248cc274296cc8dac07b2b3225b1f2b75522587ede",
        "input": "0x95e73ac4000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000462361353032393033342d323866322d343735622d396136622d3361656435633631333263342335303338333231343036663262623238366332383333646436653564616561350000000000000000000000000000000000000000000000000000",
        "nonce": "0x0",
        "senderTxHash": "0xe7840c30159f9453c9c10d1c1158f0660d913e18269edaa18e8e501868c2d26d",
        "signatures": [
            {
                "V": "0x4055",
                "R": "0xf8106cf7d58655b07faff183db65e66ca9b872f81b6e49c06b1eddf72b29864",
                "S": "0x18b939f9f82b4f565691b5f63600d1bac2d59d5099eb327486c188c745e5d147"
            }
        ],
        "to": "0x2ace2571e4fd27ac0b07e88e0705432d2b323e02",
        "transactionIndex": "0x0",
        "type": "TxTypeFeeDelegatedSmartContractExecution",
        "typeInt": 49,
        "value": "0x0"
    }
}
```

#### Parameters

* `32-byte DATA`: Hash of a transaction.

**Returns**

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

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction\#klay\_gettransactionbyhash](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction#klay_gettransactionbyhash)

