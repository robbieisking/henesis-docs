---
description: Henesis SDK를 이용하여 어떻게 Integration에서 구독한 이벤트를 받아올 수 있는지 알아봅시다.
---

# Subscribing to Events \(WebSocket\)

## henesis.yaml 설정 및 배포

henesis.yaml에 대한 자세한 정보는 [Deploy Integration](deploy-integration.md) 챕터를 참고해주세요. 여기서는 webSocket을 위해 세팅해야 할 정보만 다시 한번 살펴보겠습니다.

{% tabs %}
{% tab title="henesis.yaml" %}
```yaml
...
provider:
  type: webSocket
```
{% endtab %}
{% endtabs %}

* `type`을 `webSocket`으로 설정해주세요.

작성한 henesis.yaml을 CLI를 통해 배포해주세요.

## Henesis SDK

Henesis에서는 webSocket을 통해 쉽게 블록체인 데이터 구독할 수 있도록 SDK를 제공합니다. 현재 Javascript 언어를 지원하며 추후에 다른 언어도 지원할 계획입니다.

설치하는 법은 [링크](../installation/henesis-sdk.md)를 참조하여 주세요.

## Authentication

Henesis SDK를 이용하기 위해서는 우선 `client-id` 가 필요합니다. `client-id`는 사용자 별로  고유하게  부여된 정보이며, 아래와 같이 Henesis CLI를 이용해 조회할 수 있습니다.

```bash
$ henesis account:describe

Email: haechi@haechi.io
Name: haechi
Organization: haechi-labs
clientId: <client-id>
```

SDK 에서는 아래와 같이 `client-id`를 이용하여 데이터 구독을 위한 `Henesis` 인스턴스를 생성할 수 있습니다.

```javascript
import { EventStreamer } from '@haechi-labs/henesis-sdk-js'

const eventStreamer = new EventStreamer('client-id');
```

## Subscription

새로 생성한 인스턴스의 `subscribe` 메서드를 이용하여 `subscription` 을 만들 수 있습니다.

```javascript
// const subscription = await eventStreamer.subscribe(topic: String, options: Object)

const subscription = await eventStreamer.subscribe(
  "streamedBlock",
  {
    integrationId: "your-integration-id",
    subscriptionId: "your-subscription-id",
    ackTimeout: 30 * 1000 // optional
  }
);
```

* `topic` : 구독할 토픽입니다. 블록 단위로 이벤트를 듣기 위해서는 `streamedBlock`을 입력합니다. 즉, 특정 블록에서 듣고자 하는 컨트랙트의 이벤트가 여러 번 발생한다면 SDK는 해당 이벤트를 배열로 한번에 전달합니다.
* `options` : `subscription`생성에 필요한 정보
  * `integrationId`: 구독하려는 Integration의 Id입니다. 
  * `subscriptionId`: Henesis는 `subscription` 생성 시점부터 블록체인 데이터를 유실없이 전달합니다. 네트워크 상황 등으로 인해 연결이 끊기더라도, 재연결이 된다면 마지막으로 전달이 성공된 데이터 이후부터 다시 전송합니다. `subscriptionId`는 유저가 지정할 수 있습니다. 만약 같은 `subscriptionId`가 존재하지 않는다면 새로운 `subscription`이 생성됩니다.
  * `ackTimeout`: Acknowledge를 기다리는 시간\(단위: ms\), `ackTimeout` 내에 Henesis SDK로 부터 ACK이 전달되지 않으면 해당 메세지가 제대로 전달되지 않았다고 판단하여 다시 전송합니다. 기본 값은 10000ms 이며 범위는 10000ms  ~ 600000ms\(10s  ~ 600s\) 입니다.

{% hint style="danger" %}
한 개의 "subscriptionId"로 연결 가능 클라이언트의 수는 1개입니다. 따라서 여러 개의 클라이언트가 동일한 "subscriptionId"로 연결할 수 없습니다. 여러 개의 클라이언트에서 데이터 구독을 원한다면, 다른 "subscriptionId"로 여러 개의 subscription을 생성해야 합니다.
{% endhint %}

{% hint style="warning" %}
`"subscriptionId"`를 정수형태로 사용하고 싶다면 문자열과 함께 사용해야 합니다. 예를 들어 "1"은 허용되지 않지만, "a1"은 허용됩니다.
{% endhint %}

위에서 생성한 `subscription`을 이용하여 아래와 같이 이벤트를 구독할 수 있습니다.

```javascript
subscription.on('message', async (message) => {
  const events = message.data;
  // processing events
  // For example, you can save events to your database.
  message.ack(); // (MUST) Send an ACK message!!
});

subscription.on('error', err => {
  console.error(err);
});

subscription.on('close'. err => {
  console.error(err);
});
```

Henesis에서 전달하는 데이터를 구독하기 위해서 `on` 함수를 사용합니다. `subscription#on()` 에는 3가지 토픽이 존재합니다. 각 토픽의 데이터는 아래와 같은 상황에서 발생합니다. 각 토픽에서 전달되는 데이터를 이용해 원하는 로직을 구현할 수 있습니다. 

| Topic | 발생 상황 |
| :--- | :--- |
| message | Henesis에서 데이터를 수집했을 때 |
| error | Henesis에 장애 상황이 발생했을 때 \(네트워크 에러, 노드 에러\) |
| close | Subscription이 끊겼을 때 |

### 데이터 전송 보장 <a id="reliability"></a>

SDK를 통해 서버와의 연결이 성공적으로 이루어졌다면 Henesis는 어떤 상황에서도 데이터를 전달함을 보장합니다. 하지만 네트워크 이상\(와이파이 문제 등\)으로 인해 연결이 끊겼을 경우, 혹은 ACK 메세지를 Henesis로 보냈으나 ACK 메세지가 서버에 도달하지 않았을 경우에 중복 데이터가 전달될 수 있습니다. 따라서 전달받은 데이터에 대한 인덱스\(메세지ID 혹은 블록 넘버\)를 저장하여 중복 데이터에 대한 처리를 반드시 해주어야 합니다.

{% hint style="danger" %}
반드시 "message.ack\(\)"을 이용하여 ACK 메세지를 Henesis에 전달해야 합니다. ACK 메세지가 전송되지 않으면, Henesis는 메세지 전달에 실패했다고 판단하여 다음 데이터를 보내지 않고 똑같은 데이터를 다시 전송합니다.
{% endhint %}

## 응답 데이터 포맷

`message`토픽에서 구독하는 데이터의 포맷은 아래와 같습니다.

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

