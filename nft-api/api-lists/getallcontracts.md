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

해당 API는 토큰들의 배열을 반환하며, 각 토큰은 다음과 같은 필드를 가지고 있습니다:

* `address`: ERC721 컨트랙트의 주소
* `name`:  ERC721 컨트랙트의 이름
* `symbol`:  ERC721 컨트랙트의 토큰 심볼
* `totalSupply`:  ERC721 토큰의 총 발행량

