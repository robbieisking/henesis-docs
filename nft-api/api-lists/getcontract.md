# getContract

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/contracts/<contractAddress>
```

#### Response

```javascript
{
    "address": "0x273f7f8e6489682df756151f5525576e322d51a3",
    "name": "MyCryptoHeroes:Hero",
    "symbol": "MCHH",
    "totalSupply": 15712
}
```

#### Path Parameters

* `contractAddress` - String: ERC721コントラクトのアドレス Returns

**Returns**

* `address`: ERC721コントラクトのアドレス
* `name`:  ERC721コントラクトの名前
* `symbol`:  ERC721コントラクトのトークンシンボル
* `totalSupply`:  ERC721トークンの合計発行量

