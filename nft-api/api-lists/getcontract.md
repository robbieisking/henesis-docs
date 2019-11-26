---
description: Get a specific contract information that meets the ERC721 standard.
---

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

* `contractAddress` - String: The address of ERC721 contract

**Returns**

* `address`: The address of ERC721 contract
* `name`:  The name of ERC721 contract
* `symbol`:  The token symbol of ERC721 contract
* `totalSupply`:  The total supply of ERC721 tokens

