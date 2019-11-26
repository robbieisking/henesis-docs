# getTokensByContractAddress

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/contracts/<contractAddress>\
/tokens\?offset\=<offset>\&limit\=<limit>\&\
accountAddresses\=<accountAddress1>\, <accountAddress2>
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
        "tokenId": "40020032",
        "contractAddress": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "owner": "0x185b257aa51fdc45176cf1ffac6a0bfb5cf28afd",
        "tokenUri": "https://www.mycryptoheroes.net/metadata/hero/40020032"
      },
      ...
      {
        "tokenId": "40090027",
        "contractAddress": "0x273f7f8e6489682df756151f5525576e322d51a3",
        "owner": "0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
        "tokenUri": "https://www.mycryptoheroes.net/metadata/hero/40090027"
      }
    ]
  },
  "pagination": {
    "prevUrl": "http://api.henesis.io/nft/v1/contracts/0x273f7f8e6489682df756151f5525576e322d51a3/tokens?offset=0&limit=15&accountAddresses=0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
    "nextUrl": "http://api.henesis.io/nft/v1/contracts/0x273f7f8e6489682df756151f5525576e322d51a3/tokens?offset=30&limit=15&contractAddresses=0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e        "
  }
}
```

#### Path Parameters

* `contractAddress` - String: ERC721トークンを照会したいcontractアドレス

#### Query Parameters

* `offset` - Integer: ERC721トークンリストから返される最初の項目のオフセット（0から始まる）
* `limit` - Integer: 一度再生するERC 721トークンの数
* `accountAddresses` - List&lt;String&gt;: フィルタリングするaccountアドレスたち。このフィールドを使用すると、そのcontractのERC721トークンの指定されたaccountのERC721トークンだけ返されます。

**Returns**

**`data` : `getTokensByContractAddress` APIの応答データ**

* `address`: ERC721コントラクトのアドレス
* `name`: ERC721コントラクトの名前
* `symbol`: ERC721コントラクトのトークンシンボル
* `totalSupply`: ERC721トークンの合計発行量
* `tokens`: ERC721トークンの配列

トークンは、発行時間昇順にソートされ、それぞれのトークンは、次のようなフィールドを持っています：

* `id`: ERC721トークンのid
* `contractAddress`: ERC721コントラクトのアドレス
* `owner`: ERC721トークンの所有者
* `uri`: ERC721トークンのuri

**`pagination`: ERC721トークンリストのページネーション情報**

* `prevUrl`: このAPIリクエストの前のページurl
* `nextUrl`: このAPIリクエストの次のページurl

{% hint style="info" %}
もしprevUrlやnextUrlが存在しない場合、 ""で表示されます。
{% endhint %}

