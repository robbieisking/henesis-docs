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

* `contractAddress` - String: ERC721 컨트랙트의 주소

**Returns**

* `address`: ERC721 컨트랙트의 주소
* `name`:  ERC721 컨트랙트의 이름
* `symbol`:  ERC721 컨트랙트의 토큰 심볼
* `totalSupply`:  ERC721 토큰의 총 발행량

