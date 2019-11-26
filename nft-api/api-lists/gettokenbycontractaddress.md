---
description: Get a list of NFTs of an ERC721 contract.
---

# getTokensByContractAddress

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/contracts/<contractAddress>/tokens?page=<page>&size=<size>&order_by=<order_by>&order_direction=<order_direction>&accountAddresses=<accountAddress1>,<accountAddress2>
```

#### Response

```javascript
{
  "data": {
    "address": "0x273f7f8e6489682df756151f5525576e322d51a3",
    "name": "MyCryptoHeroes:Hero",
    "symbol": "MCHH",
    "totalSupply": 15712,
    "tokens": [
      {
        "id": "40020032",
        "contractAddress": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "owner": "0x185b257aa51fdc45176cf1ffac6a0bfb5cf28afd",
        "uri": "https://www.mycryptoheroes.net/metadata/hero/40020032"
      },
      ...
      {
        "id": "40090027",
        "contractAddress": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "owner": "0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
        "uri": "https://www.mycryptoheroes.net/metadata/hero/40090027"
      }
    ]
  },
  "pagination": {
    "prevUrl": "http://api.henesis.io/nft/v1/contracts/0x273f7f8e6489682df756151f5525576e322d51a3/tokens?page=0&size=15&order_by=transfer_block_number&order_direction=desc&accountAddresses=0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
    "nextUrl": "http://api.henesis.io/nft/v1/contracts/0x273f7f8e6489682df756151f5525576e322d51a3/tokens?page=2&size=15&order_by=transfer_block_number&order_direction=desc&accountAddresses=0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e        "
  }
}
```

#### Path Parameters

* `contractAddress` - String: The contract address to retrieve ERC721 tokens

#### Query Parameters

* `page` - Integer: The page of results to return.
* `size` - Integer: The number of ERC721 tokens to return in one request, specified as an integer from 1 to 200.
* `order_by` - String: The field by which the results is sorted \(default: `transfer_block_number` \)
  * `transfer_block_number` : Based on the block number where the transfer occurred
* `order_direction` - String: The order direction \(default: `desc`\)
  * `asc`: Ascending order
  * `desc`: Descending order
* `accountAddresses` - List&lt;String&gt;: The contract addresses to filter. This field returns only the ERC721 tokens of the specified contract among the tokens owned by the account 

**Returns**

**`data` : The response of `getTokensByContractAddress` API call**

* `address`: The address of ERC721 contract
* `name`: The name of ERC721 contract
* `symbol`: The symbol of ERC721 token
* `totalSupply`: The totalSupply of ERC721 token 
* `tokens`: The array of ERC721 tokens

Tokens are sorted in ascending order of issue time, and each token has the following fields:

* `id`: The id of ERC721 token
* `contractAddress`: The address of ERC721 contract
* `owner`: The owner of ERC721 token
* `uri`: The uri of ERC721 token

**`pagination`: The pagination information of ERC721 token list**

* `prevUrl`: The previous url of this API request
* `nextUrl`: The next url of this API request

{% hint style="info" %}
If `prevUrl` or `nextUrl` does not exist, it would be displayed as "".
{% endhint %}

