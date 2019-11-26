# getTokensByAccountAddress

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/accounts/<accountAddress>\
/tokens\?offset\=<offset>\&limit\=<limit>\&\
contractAddresses\=<contractAddress1>\, <contractAddress2>
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
    "prevUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?offset=0&limit=15&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3",
    "nextUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?offset=30&limit=15&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3"
  }
}
```

#### Path Parameters

* `accountAddress` - ERC721トークンを照会したいaccountアドレス

#### Query Parameters

* `offset` - Integer: ERC721トークンリストから返される最初の項目のオフセット（0から始まる）
* `limit` - Integer: 一度再生するERC 721トークンの数
* `contractAddresses` - List&lt;String&gt;: フィルタリングするcontractアドレスたち。このフィールドを使用すると、accountが所有してトークンの指定されたcontractのERC721トークンだけ返されます。

**Returns**

**`data` : ERC721トークンの配列**

トークンは、発行時間昇順にソートされ、それぞれのトークンは、次のようなフィールドを持っています：

* `id`: ERC721トークンのid
* `owner`: ERC721トークンの所有者
* `uri`: ERC721トークンのuri
* `contract`: ERC721 contract情報

それぞれのcontractは、次のようなフィールドを持っています：

* `address`: ERC721コントラクトのアドレス
* `name`: ERC721コントラクトの名前
* `symbol`: ERC721コントラクトのトークンシンボル
* `totalSupply`: ERC721トークンの合計発行量

**`pagination`: ERC721トークンリストのページネーション情報**

* `prevUrl`: このAPIリクエストの前のページurl
* `nextUrl`: このAPIリクエストの次のページurl

{% hint style="info" %}
もしprevUrlやnextUrlが存在しない場合、 ""で表示されます。
{% endhint %}

