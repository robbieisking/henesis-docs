---
description: Get a list of all contracts that meet the ERC721 standard.
---

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

It returns the array of tokens, and each token has the following fields:

* `address`: The address of ERC721 contract
* `name`:  The name of ERC721 contract
* `symbol`:  The token symbol of ERC721 contract
* `totalSupply`:  The total supply of ERC721 contract

