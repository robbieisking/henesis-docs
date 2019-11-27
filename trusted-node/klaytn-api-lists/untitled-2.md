---
description: >-
  Returns information about a block by block number. This API works only on RPC
  call, not on Javascript console.
---

# klay\_getBlockByNumber

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getBlockByNumber","params":["0x1b4", true],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockscore": "0x1",
        "extraData": "0xd883010000846b6c617988676f312e31322e35856c696e757800000000000000f9053ff9018f940b59cae1f03534209fdb9ddf5ea65b310cd7060c9416c192585a0ab24b552783b4bf7d8dc9f6855c35941782834bf8847e235f21f2c1f13fca4d5dff6621942bdd279522b8a0843831fbb94cfbb24a913597c594386ca3cb8bb13f48d1a6adc1fb8df09e7bb7f9c89445ed4bf508ca2061a8b4a16e278ea85a8f87a70b9452d41ca72af615a1ac3301b0a93efa222ecc75419456e8c1463c341abf8b168c3079ea41ce8a387e189456e3a565e31f8fb0ba0b12c03355518c6437212094bca8ffa45cc8e30bbc0522cdf1a1e0ebf540dfe294e783fc94fddaeebef7293d6c5864cff280f121e194e93a890fb7ec5e993b1a7fd77b0d13a0763eff3d94ed6ee8a1877f9582858dbe2509abb0ac33e5f24e94f6623192aa096e192024ecb381b04c58df9a607594a2ba8f7798649a778a1fd66d3035904949fec55594e3d92072d8b9a59a0427485a1b5f459271df457c94ec6c1cede510be308f0fdbbc8dbdf238829bdb3494f113ec8c22765d485309cf1d025d1b975245b9f894f8c9c61c5e7f2b6219d1c28b94e5cb3cdc802594b8417a3b093d8e7818724ab99524cfd6ce95e4ba270afd4cefa387de4ddd321bc301149b56ab614f0780f187dd3efbc32ea9a18c284192477a852e30517e7ab7f41d01f90367b841307e36f790baf330b8b0a66761f0c226a5444b8ce3491a1e93c645de4f405bc16225abf267ba502cf32c6f566eb7320eb271a226e869ecfcd5e9e717f71cdb5800b841082e28628cb0e5d622cc1af1a36e6068ecd9e0461abfb49990786db9bbd50a401b52ead25eb27e7926fcf5194e1a649cfb7ab727ada8d8e297ac896be7fe125900b841bf513d3fcbdcb2dcde6f182d2af0a7e3c87e6978ed90a53dedb5353372f4ffa0288efbd593ef964a1440d4c0fa963760d373e5fc28fb2c919b5ce5f47aa35c0c01b841da915624b30cf66aac954e1f665bfc65dd6202972548b816d55362dc9d4b909f552938e1d7ac548c59450acef888c59421d0f418df75c38d88b0f9eb3d97e8a400b8414d62c68cd4e3ec09f95b81be3c0099d62c8198940515b504dd0236b81ee3b2656f8d46489b5297a61672098231d6f119fde8afc6dfb23db27116535580a36fb800b841792e14c7f3d8df40102327f12ac29b9d624b383d7dcd26c8089bdc68313fc5663a714de5b87b97ccfe6d3ba7ce8fbc9dc320523927261d8976ac334cf21301a301b841f4d60aab396357f5bbf790a424ac7a72186943644b54b31a1943e5a43cfdad212426526d410a307a67d047c7dee79829a56ffc7f3b726c7a053b756c1881959c01b841b01c29cffc120ff8dacc7bc78f7d7e09596c58c01841c266bd267ec2e3bf5c8e4123667c0cdeeafd4fadcb5e1babe60359f9472de9dc1ef2d7c84d574aa71f0200b8419b8fa1d532578646ab7d60e51332bf01f5c167be1a6edcf0ae792394ccda43e2289bd272406ca5c77fcef70673feeabc59eb57746c2b6116462e0b919bf022dd00b84103cf8e227dced6d30a4380d75abe4a13891faae5df79d006ba4de6374730ca296d9ac6a5aa6f73ef31d5422f6214b995d8971f665a0b37a08ae06a308211ee8700b8417bc2dca8dcdd2302025db4b484ebbfa8415d0c6643173988c68e051b63d0318a641bf784e679f357d0939aa59b5dd5beddfcbef7dd02d93ea49690c80cbb785900b8411ae2c4320b85d0a96fe5d8cdbe191ae103b3ff2c1cf0f13f98433e9447d4d3a90d8331e85a67de32c562f94b67736c56204036f7b90317709896498bd9e81f5900b841b61ce6294f0a1572d2a2a7e5fa5893875111dd6d38680515f5149ac35ae8b60c394c83666242ee8ab79499c36f5eb38e99a90c9538044ae87d8dca232ada532401",
        "gasUsed": "0x0",
        "governanceData": "0x",
        "hash": "0x6080a19cb3d68efdf90412cb621a92b41d28422a2db9db6db37d6de41c58d983",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "number": "0x1b4",
        "parentHash": "0x766ee2af3da0cb59e6ecc0d6a1152041151feed60a1ecfb2ae1876bb1a99e350",
        "receiptsRoot": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "reward": "0xbc8d2fd8c2ab4b76306d9d8b9f5721b13f239220",
        "size": "0x715",
        "stateRoot": "0x90e1f9a58bfda386422886d4f2e43f21094826787a124eacb1e0df8017b52cfe",
        "timestamp": "0x5d1209f6",
        "timestampFoS": "0x20",
        "totalBlockScore": "0x1b5",
        "transactions": [],
        "transactionsRoot": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "voteData": "0x"
    }
}
```

#### Parameters

* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`
* `BOOLEAN`: If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

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

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getblockbynumber](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getblockbynumber)

