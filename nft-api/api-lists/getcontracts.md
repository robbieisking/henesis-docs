# getAllContracts

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/contracts/
```

#### Response

```javascript
[
    {
        "address": "0x0e0466ebd9078478cdb64de5ea4bf0982da6fe5b",
        "name": "CryptoHuskies",
        "symbol": "CH",
        "totalSupply": 8734
    },
    ...
    {
        "address": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "name": "MyCryptoHeroes:Hero",
        "symbol": "MCHH",
        "totalSupply": 15712
    },
]
```

**Returns**

このAPIはトークンの配列を返します。そして、各トークンは以下のようなフィールドを持っています：

* `address`: ERC721コントラクトのアドレス
* `name`:  ERC721コントラクトの名前
* `symbol`:  ERC721コントラクトのトークンシンボル
* `totalSupply`:  ERC721トークンの合計発行量

