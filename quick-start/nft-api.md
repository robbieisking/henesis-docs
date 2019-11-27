# NFT API

## NFT API 사용하기

`curl` 명령어를 통해 NFT API를 사용할 수 있습니다 .

```bash
curl -X GET http://api.henesis.io/nft/v1/accounts/<accountAddress>\
/tokens\?page\=<page>\&size\=<size>\&direction\=<direction>\&
contractAddresses\=<contractAddress1>\, <contractAddress2>
```

위의 api 요청을 통해 ERC721 토큰 정보를 요청하면, 아래와 같은 응답을 받을 수 있습니다.

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
    "nextUrl": "http://api.henesis.io/nft/v1/accounts/0x138a35ee20e40f019e7e7c00386ab2ef42d66d1e/tokens?page=0&size=15&direction=ASC&contractAddresses=0x273f7f8e6489682df756151f5525576e322d51a3"
  }
}
```
