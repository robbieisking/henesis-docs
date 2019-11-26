---
description: Webhook을 이용해 어떻게 Integration에서 수집한 데이터를 받아올 수 있는지 알아봅시다.
---

# Subscribing to Events \(Webhook\)

## Webhook 이란?

Webhook은 Integration에서 블록체인 데이터를 수집했을 때 데이터를 HTTP 호출 방식으로 전달하는 개념입니다. Webhook 방식으로 데이터를 받기 위해서 서버 URL을 제공해야 합니다. Henesis에서는 블록체인 데이터가 수집되었을 때 제공된 URL에 HTTP 호출을 합니다. 지속적으로 데이터를 폴링\(Polling\)하는 것보다 새롭게 데이터가 생성되었을 때 데이터를 푸시\(Push\)하는 Webhook 방식이 리소스 사용에 더 효율적입니다.

## ngrok 세팅

{% hint style="info" %}
ngrok은 손쉽게 외부에 노출할 수 있는 URL을 만들어주는 서비스입니다. 해당 URL로 들어온 트래픽을 내부 네트워크에 포워딩할 수 있습니다. 고정 IP가 없는 로컬 환경에서 간단하게 webhook이 잘 동작하는지 확인 때에 유용합니다.
{% endhint %}

* [ngrok을 다운로드](https://ngrok.com/download) 합니다.
* \(Linux\) 아래 명령어를 통해 ngrok 압축 해제합니다.

```bash
$ unzip /path/to/ngrok.zip
```

* [ngrok에서 회원가입](https://dashboard.ngrok.com/signup) 후, 인증합니다. 

```bash
$ ./ngrok authtoken <YOUR_AUTH_TOKEN>
```

* 로컬 호스트\(localhost\)의 3000 포트를 External IP로 노출 시킵니다.

```bash
$ ./ngrok http 3000
```

* 다음과 같은 화면이 출력되는 지 확인합니다.  화면에 보이는 **http://xxxx.ngrok.io 를 클립보드에 복사**합니다

```bash
ngrok by @inconshreveable                                                                                                                                                             (Ctrl+C to quit)

Session Status                online
Account                        (Plan: Free)
Version                       2.3.29
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://xxxx.ngrok.io -> http://localhost:3000
Forwarding                    https://xxxx.ngrok.io -> http://localhost:3000

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

## 서버 구현  

[Express](https://expressjs.com/)를 이용해 Node 기반의 Webhook 서버를 아래와 같이 만듭니다.

```bash
$ npm install express --save
```

{% code title="server.js" %}
```javascript
const express = require('express');
const app = express();
// port는 ngrok에서 설정한 port와 동일해야합니다.
const port = 3000;

app.use(express.json());
app.post('/webhook', (req, res) => {
    console.log(JSON.stringify(req.body, null, '  '));
    res.send('Received')
});
app.listen(port, () => {
    console.log(`Application listening on port ${port}!`)
});
```
{% endcode %}

`server.js` 를 이용해 Henesis가 HTTP POST로 호출할 `/webhook` URL을 제공할 수 있습니다.

## henesis.yaml 설정 및 배포

henesis.yaml에 대한 자세한 정보는 [Deploy Integration](deploy-integration.md) 챕터를 참고해주세요. 여기서는 webhook을 위해 세팅해야 할 정보만 다시 한번 살펴보겠습니다.

{% code title="henesis.yaml" %}
```yaml
...
provider:
  type: webhook
  url: http://xxxx.ngrok.io/api/webhook
  method: POST
  headers: 
    Authorization: 'Bearer YOUR-OWN-TOKEN'
```
{% endcode %}

* `type`을 `webhook`으로 설정해주세요.
* `webhook`을 받을 `url`과 `method`를 설정해주세요.
* HTTP header가 필요하다면 `headers`에 입력해주세요. \(ex. Authorization\)

작성한 henesis.yaml을 CLI를 통해 배포해주세요.

## 실행 

앞서 작성한 `server.js` 로 Node 서버를 실행해봅니다. 

```bash
$ node server.js
```

그 다음, 콘솔창에서 블록체인 메세지가 올바르게 출력되는지 확인합니다. 

{% hint style="danger" %}
네트워크 이상\(와이파이 문제 등\)으로 인해 중복된 메세지가 전달될 수 있습니다. 따라서 전달받은 데이터에 대한 인덱스\(메세지ID 혹은 블록 넘버\)를 저장하여 중복 데이터에 대한 처리를 반드시 해주어야 합니다.
{% endhint %}

Webhook으로 구독하는 메세지의 포멧은 아래와 같습니다.

```javascript
{
  "events": [
    {
      "contractAddress": "0x20a1cff753ae36988f73507a5497f02369970d70",
      "contractName": "KittyCore",
      "eventName": "Transfer(address,address,uint256)",
      "data": [
        {
          "name": "from",
          "value": "0x1A96cDf0Beb4DcD61c0Fd2D5d0E852DF9E631b69",
          "type": "address"
        },
        {
          "name": "to",
          "value": "0xC1C88A7B90062866F54320b18Ee5DA65F79202D6",
          "type": "address"
        },
        {
          "name": "tokenId",
          "value": "130",
          "type": "uint256"
        }
      ],
      "transaction": {
        "blockHash": "0x6267f7e3db589571ca25ac6dfc8dd78db85215c722033dd129ee001d3d87d0b2",
        "blockNumber": "0x890ba8",
        "from": "0x1a96cdf0beb4dcd61c0fd2d5d0e852df9e631b69",
        "gas": "0x771000",
        "gasPrice": "0x5d21dba00",
        "hash": "0xd07f955010a0049e9169eac94f0312a3a9dd6ea108e01dd2efcba81af0bf836d",
        "input": "0x4ad8c938000000000000000000000000000000000000000000000000000000000000008200000000000000000000000000000000000000000000000000000000000186a000000000000000000000000000000000000000000000000000000000000186a10000000000000000000000000000000000000000000000000000000000000064",
        "nonce": "0x4f3",
        "senderTxHash": "0xd07f955010a0049e9169eac94f0312a3a9dd6ea108e01dd2efcba81af0bf836d",
        "signatures": [
          {
            "V": "0x7f5",
            "R": "0xd63bf59c294bfd26e0f756b374af7c2b09e89a1b75bbe16d8a8999b96c97271d",
            "S": "0x64dae73afc0992b87a0a23275c00f8898681c621c6203d6a36fa427af3b0ca7e"
          }
        ],
        "to": "0x20a1cff753ae36988f73507a5497f02369970d70",
        "transactionIndex": "0x0",
        "type": "TxTypeLegacyTransaction",
        "typeInt": 0,
        "value": "0x0"
      }
    },
    {
      "contractAddress": "0xc1c88a7b90062866f54320b18ee5da65f79202d6",
      "contractName": "SiringClockAuction",
      "eventName": "AuctionCreated(uint256,uint256,uint256,uint256)",
      "data": [
        {
          "name": "tokenId",
          "value": "130",
          "type": "uint256"
        },
        {
          "name": "startingPrice",
          "value": "100000",
          "type": "uint256"
        },
        {
          "name": "endingPrice",
          "value": "100001",
          "type": "uint256"
        },
        {
          "name": "duration",
          "value": "100",
          "type": "uint256"
        }
      ],
      "transaction": {
        "blockHash": "0x6267f7e3db589571ca25ac6dfc8dd78db85215c722033dd129ee001d3d87d0b2",
        "blockNumber": "0x890ba8",
        "from": "0x1a96cdf0beb4dcd61c0fd2d5d0e852df9e631b69",
        "gas": "0x771000",
        "gasPrice": "0x5d21dba00",
        "hash": "0xd07f955010a0049e9169eac94f0312a3a9dd6ea108e01dd2efcba81af0bf836d",
        "input": "0x4ad8c938000000000000000000000000000000000000000000000000000000000000008200000000000000000000000000000000000000000000000000000000000186a000000000000000000000000000000000000000000000000000000000000186a10000000000000000000000000000000000000000000000000000000000000064",
        "nonce": "0x4f3",
        "senderTxHash": "0xd07f955010a0049e9169eac94f0312a3a9dd6ea108e01dd2efcba81af0bf836d",
        "signatures": [
          {
            "V": "0x7f5",
            "R": "0xd63bf59c294bfd26e0f756b374af7c2b09e89a1b75bbe16d8a8999b96c97271d",
            "S": "0x64dae73afc0992b87a0a23275c00f8898681c621c6203d6a36fa427af3b0ca7e"
          }
        ],
        "to": "0x20a1cff753ae36988f73507a5497f02369970d70",
        "transactionIndex": "0x0",
        "type": "TxTypeLegacyTransaction",
        "typeInt": 0,
        "value": "0x0"
      }
    }
  ],
  "blockMeta": {
    "platform": "klaytn",
    "network": "baobab",
    "blockNumber": 8981416,
    "blockHeader": {
      "blockscore": "0x1",
      "extraData": "0xd883010101846b6c617988676f312e31322e35856c696e757800000000000000f90164f85494571e53df607be97431a5bbefca1dffe5aef56f4d945cb1a7dccbd0dc446e3640898ede8820368554c89499fb17d324fa0e07f23b49d09028ac0919414db694b74ff9dea397fe9e231df545eb53fe2adf776cb2b84114b9795ff81d5af6c190bf5ae962a6ed4e76e74fbbc447c2f70d358f3dacfe1046fbae97057a368878c84f6a944f3c5b37e80b22714dcbf58db47e0623c1318400f8c9b841ef014b5e074b6d5fa66948e7cc6ef38e70248c58f8ed915fb2a7a23cb41c2e107fcf339b7100dba8af2a8e68c012d403c5ad9c25ffb198d73bba105c6cb8099b01b8414c8b22ab16965f088f0624aa824f5949a2632f1c31690616f0467f37e34882df4cc74fef283bbaa63c74ffd853b2dc08cab8814af0f4d05b574baaa291078b3c01b841a86eee4611709e2fe39a71e34a39411a394a70941aa1c80b74dff90710291ab3322a0e5f3f1942feca148d2a1f9cc60cfbb600e573b12283c5eab751ef21267901",
      "gasUsed": 140703,
      "governanceData": "0x",
      "hash": "0x6267f7e3db589571ca25ac6dfc8dd78db85215c722033dd129ee001d3d87d0b2",
      "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000000000000000000000000808000000000000000100000000000000000000000000000000000000000040000000000000000000000000000000000010000000000000000000000000000002000000000000000000000000000000000000000000000400000000000000000000000000000000000000010000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000",
      "number": 8981416,
      "parentHash": "0xdb469243e1a98501c8bdf5278c9bd7c55df12fc057c561ca41f04137015d7cb6",
      "receiptsRoot": "0x82359dd8426d89949c393037c6056743401b8e1a48de6971ecc02ee2882e2e62",
      "reward": "0xa86fd667c6a340c53cc5d796ba84dbe1f29cb2f7",
      "size": 1071,
      "stateRoot": "0x87dc27fe65791184493bf4032155c32c20bae00eb79ac269ae1141ee23b135bf",
      "timestamp": 1570526845,
      "timestampFoS": "0xc",
      "totalBlockScore": "0x890ba9",
      "transactions": [
        "0xd07f955010a0049e9169eac94f0312a3a9dd6ea108e01dd2efcba81af0bf836d"
      ],
      "transactionsRoot": "0xd07f955010a0049e9169eac94f0312a3a9dd6ea108e01dd2efcba81af0bf836d",
      "voteData": "0x"
    }
  },
  "userMeta": {
    "integrationId": "kitty-tutorial-klaytn-uijkh",
    "userId": 1
  }
}
```

