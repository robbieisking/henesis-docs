---
description: Returns receipts included in a block identified by block hash.
---

# klay\_getBlockReceipts

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0", "method":"klay_getBlockReceipts", "params":["0x348f0b61223c046b395d8f4c99be394384cb8034d83c6de55890ea5aa62ddc31"],"id":73}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 73,
    "result": [
        {
            "blockHash": "0x348f0b61223c046b395d8f4c99be394384cb8034d83c6de55890ea5aa62ddc31",
            "blockNumber": "0xb2e6ff",
            "contractAddress": null,
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
            "gasUsed": "0x42197",
            "input": "0x95e73ac4000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000462361353032393033342d323866322d343735622d396136622d3361656435633631333263342335303338333231343036663262623238366332383333646436653564616561350000000000000000000000000000000000000000000000000000",
            "logs": [
                {
                    "address": "0x2ace2571e4fd27ac0b07e88e0705432d2b323e02",
                    "topics": [
                        "0x57a8831b6db665c2f5cab7d8475251e7ed6a00890f43f612afa7c3cc34788a57"
                    ],
                    "data": "0x000000000000000000000000ecf2d7047dc7634557840cd205f2c6853d022e88000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000462361353032393033342d323866322d343735622d396136622d3361656435633631333263342335303338333231343036663262623238366332383333646436653564616561350000000000000000000000000000000000000000000000000000",
                    "blockNumber": "0xb2e6ff",
                    "transactionHash": "0x716aa19341369daa04bc15248cc274296cc8dac07b2b3225b1f2b75522587ede",
                    "transactionIndex": "0x0",
                    "blockHash": "0x348f0b61223c046b395d8f4c99be394384cb8034d83c6de55890ea5aa62ddc31",
                    "logIndex": "0x0",
                    "removed": false
                }
            ],
            "logsBloom": "0x00000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000400000000000000000000000000002000000000000000000000000000000000000000000000000000",
            "nonce": "0x0",
            "senderTxHash": "0xe7840c30159f9453c9c10d1c1158f0660d913e18269edaa18e8e501868c2d26d",
            "signatures": [
                {
                    "V": "0x4055",
                    "R": "0xf8106cf7d58655b07faff183db65e66ca9b872f81b6e49c06b1eddf72b29864",
                    "S": "0x18b939f9f82b4f565691b5f63600d1bac2d59d5099eb327486c188c745e5d147"
                }
            ],
            "status": "0x1",
            "to": "0x2ace2571e4fd27ac0b07e88e0705432d2b323e02",
            "transactionHash": "0x716aa19341369daa04bc15248cc274296cc8dac07b2b3225b1f2b75522587ede",
            "transactionIndex": "0x0",
            "type": "TxTypeFeeDelegatedSmartContractExecution",
            "typeInt": 49,
            "value": "0x0"
        }
    ]
}
```

#### Parameters

* `32-byte DATA`: Block hash

#### Returns

Receipts included in a block. If the target block contains no transaction, an empty array `[]` is returned.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getblockreceipts](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getblockreceipts)

