---
description: Get a list of NFTs owned by the account.
---

# getTokensByAccountAddress

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/accounts/<accountAddress>/tokens?page=<page>&size=<size>&order_by=<order_by>&order_direction=<order_direction>&contractAddresses=<contractAddress1>,<contractAddress2>
```

#### Response

```javascript
{
  "data": [
    {
      "id": "40020079",
      "owner": "0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
      "uri": "https://www.mycryptoheroes.net/metadata/hero/40020079",
      "contract": {
        "address": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "name": "MyCryptoHeroes:Hero",
        "symbol": "MCHH",
        "totalSupply": 15712
      }
    },
    ...
    {
      "id": "40060029",
      "owner": "0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
      "uri": "https://www.mycryptoheroes.net/metadata/hero/40060029",
      "contract": {
        "address": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "name": "MyCryptoHeroes:Hero",
        "symbol": "MCHH",
        "totalSupply": 15712
      }
    }
  ],
  "pagination": {
    "prevUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?page=0&size=15&order_by=transfer_block_number&order_direction=desc&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3",
    "nextUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?page=2&size=15&order_by=transfer_block_number&order_direction=desc&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3"
  }
}
```

#### Path Parameters

* `accountAddress` - String: The address of account for which you want to retrieve ERC721 tokens

#### Query Parameters

* `page` - Integer: The page of results to return.
* `size` - Integer: The number of ERC721 tokens to return in one request, specified as an integer from 1 to 200.
* `order_by` - String: The field by which the results is sorted \(default: `transfer_block_number` \)
  * `transfer_block_number` : Based on the block number where the transfer occurred 
* `order_direction` - String: The order direction \(default: `desc`\)
  * `asc`: Ascending order
  * `desc`: Descending order
* `contractAddresses` - List&lt;String&gt;: The contract addresses to filter This field returns only the ERC721 tokens of the specified contract among the tokens owned by the account

**Returns**

**`data` : The array of ERC721 tokens**

Each token has the following fields:

* `id`: The id of ERC721 token
* `owner`: The owner of ERC721 token
* `uri`: The uri of ERC721 token
* `contract`: The information of ERC721 contract

Each contract has the following fields:

* `address`: The address of ERC721 contract
* `name`: The name of ERC721 contract
* `symbol`: The symbol of ERC721 token
* `totalSupply`: The totalSupply of ERC721 token 

**`pagination`: The pagination information of ERC721 token list**

* `prevUrl`: The previous url of this API request
* `nextUrl`: The next url of this API request

{% hint style="info" %}
If `prevUrl` or `nextUrl` does not exist, it would be displayed as "".
{% endhint %}

