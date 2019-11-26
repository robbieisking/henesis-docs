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

* `contractAddress` - String: ERC721 토큰을 조회하고 싶은 contract 주소

#### Query Parameters

* `page` - Integer: 반환되는 결과 페이지입니다.
* `size` - Integer: 한 번의 요청으로 리턴되는 ERC721 토큰의 개수 1부터 200 사이의 정수로 지정합니다.
* `order_by` - String: 반환 결과를 정렬 할 필드 \(default : `transfer_block_number`\)
  * `transfer_block_number` : transfer가 발생한 블록 넘버 기준
* `order_direction` - String: 토큰의 발생 시간을 기준으로 정렬 방식 \(default: `desc`\)
  * `asc`: 오름차순 정렬
  * `desc`: 내림차순 정렬
* `accountAddresses` - List&lt;String&gt;: 필터링할 account 주소들. 이 필드를 이용하면 해당 contract의 ERC721 토큰 중 지정된 account의 ERC721 토큰들만 반환됩니다.

**Returns**

**`data` :  `getTokensByContractAddress` API의 응답 데이터**

* `address`: ERC721 컨트랙트의 주소
* `name`:  ERC721 컨트랙트의 이름
* `symbol`:  ERC721 컨트랙트의 토큰 심볼
* `totalSupply`:  ERC721 토큰의 총 발행량
* `tokens`: ERC721 토큰들의 배열

토큰은 발행 시간 오름차순으로 정렬되며, 각각의 토큰은 다음과 같은 필드를 가지고 있습니다:

* `id`: ERC721 토큰의 id
* `contractAddress`: ERC721 컨트랙트의 주소
* `owner`: ERC721 토큰의 소유자
* `uri`: ERC721 토큰의 uri

**`pagination`: ERC721 토큰 리스트의 페이지네이션 정보**

* `prevUrl`: 해당 API 요청의 이전 페이지 url
* `nextUrl`: 해당 API 요청의 다음 페이지 url

{% hint style="info" %}
만약 `prevUrl` 이나 `nextUrl` 이 존재하지 않는다면, ""으로 표시됩니다.
{% endhint %}

