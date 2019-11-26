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
    "prevUrl": "http://api.henesis.io/nft/v1/contracts/0x273f7f8e6489682df756151f5525576e322d51a3/tokens?page=0&size=15&order_by=transfer_block_number&order_direction=desc&accountAddresses=0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e",
    "nextUrl": "http://api.henesis.io/nft/v1/contracts/0x273f7f8e6489682df756151f5525576e322d51a3/tokens?page=2&size=15&order_by=transfer_block_number&order_direction=desc&contractAddresses=0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e        "
  }
}
```

#### Path Parameters

* `contractAddress` - String: ERC721トークンを検索したいcontractのアドレス

#### Query Parameters

* `page` - Integer: 返される結果のページです
* `size` - Integer: 一回のリクエストで戻されるERC721トークンの数、1から200の間の整数で指定します。
* `order_by` - String: 戻り値の結果の並べ替えフィールド \(default: `transfer_block_number` \)
  * `transfer_block_number` : 転送が発生したブロック番号の基準
* `order_direction` - String:  並べ替えの方向 \(default: `desc`\)
  * `asc`: 昇順で並べ替え
  * `desc`: 降順の並べ替え
* `accountAddresses` - List&lt;String&gt;: フィルタリングするcontractアドレスたち。このフィールドを使用することで、アカウントが所有しているトークンのうち、指定されたcontractのERC721トークンだけが返されます。

**Returns**

**`data` : `getTokensByContractAddress` APIのレスポンスデータ**

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

