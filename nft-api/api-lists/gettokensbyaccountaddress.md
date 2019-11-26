# getTokensByAccountAddress

#### Request

```bash
curl -X GET http://api.henesis.io/nft/v1/accounts/<accountAddress>\
/tokens\?page\=<page>\&size\=<size>\&direction\=<direction>\&
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
    "prevUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?page=0&size=15&direction=ASC&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3",
    "nextUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?page=2&size=15&direction=ASC&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3"
  }
}
```

#### Path Parameters

* `accountAddress` - String: ERC721 토큰을 조회하고 싶은 account 주소

#### Query Parameters

* `page` - Integer: ERC721 토큰 리스트에서 반환될 항목의 페이지 \(0부터 시작\)
* `size` - Integer: 한번에 조회할 ERC 721 토큰의 개수
* `direction` - String: 토큰의 발생 시간을 기준으로 정렬 방식 \(default: 내림차순\)
  * `ASC`또는 `asc`: 오름차순 정렬
  * `DES`또는 `des`: 내림차순 정렬
* `contractAddresses` - List&lt;String&gt;: 필터링할 contract 주소들. 이 필드를 이용하면 account가 소유한 토큰 중 지정된 contract의 ERC721 토큰들만 반환됩니다.

**Returns**

**`data` : ERC721 토큰들의 배열**

각각의 토큰은 다음과 같은 필드를 가지고 있습니다:

* `id`: ERC721 토큰의 id
* `owner`: ERC721 토큰의 소유자
* `uri`: ERC721 토큰의 uri
* `contract`: ERC721 contract 정보

각각의 contract는 다음과 같은 필드를 가지고 있습니다:

* `address`: ERC721 컨트랙트의 주소
* `name`:  ERC721 컨트랙트의 이름
* `symbol`:  ERC721 컨트랙트의 토큰 심볼
* `totalSupply`:  ERC721 토큰의 총 발행량

**`pagination`: ERC721 토큰 리스트의 페이지네이션 정보**

* `prevUrl`: 해당 API 요청의 이전 페이지 url
* `nextUrl`: 해당 API 요청의 다음 페이지 url

{% hint style="info" %}
만약 `prevUrl` 이나 `nextUrl` 이 존재하지 않는다면, ""으로 표시됩니다.
{% endhint %}

