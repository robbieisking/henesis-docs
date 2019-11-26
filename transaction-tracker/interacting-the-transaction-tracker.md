---
description: Henesis를 이용해 SDK만으로 어떻게 간단하게 트랜잭션의 상태를 추적할 수 있는지 알아보자.
---

# Interacting with the Transaction Tracker

## Henesis SDK

Henesis SDK를 통해 트랜잭션의 상태를 추적할 수 있습니다. 현재 SDK는 javascript 언어를 지원하며 추후에 지원 언어를 확장할 계획입니다.

설치하는 법은 [링크](../installation/henesis-sdk.md)를 참조하여 주세요.

## Authentication

Henesis SDK를 이용하기 위해서는 `client-id` 가 필요합니다. `client-id`는 계정 별로 고유하게 부여된 정보이며, 아래와 같이 Henesis CLI를 이용해 조회할 수 있습니다.

```bash
$ henesis account:describe

Email: haechi@haechi.io
Name: haechi
Organization: haechi-labs
ClientId: <client-id>
```

SDK 에서는 아래와 같이 `client-id`를 이용하여 트랜잭션 상태 추적을 위한 인스턴스를 생성할 수 있습니다.

```javascript
import { TransactionTracker } from '@haechi-labs/henesis-sdk-js'

const transactionTracker = new TransactionTracker('client-id', {
    // platform and network options
    platform: 'ethereum',
    network: 'ropsten'
});
```

## Select Platform and Network

`client-id`로 인증을 하고 나서 사용하고자 하는 `platform`과 `network`를 선택합니다.

```typescript
import { TransactionTracker } from '@haechi-labs/henesis-sdk-js'

const transactionTracker = new TransactionTracker('client-id', {
    // platform and network options
    platform: 'ethereum',
    network: 'ropsten'
});
```

Henesis에서 지원되는 `platform`과 `network`는 [여기](https://docs.henesis.io/v/ko/faq/supported-blockchains)에서 확인가능합니다.

## Tracking Transactions

트랜잭션 상태를 추적하기 위해서는 트랜잭션을 발생시킬 때에 생성된 `txHash`를 사용합니다. 생성한 `txHash`를 SDK에서 제공되는 `trackTransaction` 함수 이용해 Henesis에게 해당 트랜잭션을 추적하도록합니다.

```javascript
const txHash = await web3.eth.sendRawTransaction(sign(privateKey, txData))

henesis.trackTransaction(txHash, {
	"confirmation": 12,
	"timeout": 30 * 1000,
});
```

* `confirmation`: 해당 트랜잭션이 담긴 블록 뒤에 몇 개의 블록이 더 생성되어야  _confirmation_ 상태로 인정할 것인지 설정할 수 있습니다. 일반적으로 12 이상으로 하면 chain reorganization이 일어날 확률은 거의 없습니다. 
* `timeout`:  트랜잭션이 _receipt_ 상태 바뀌는 것을 얼마동안 기다릴지 정합니다. 해당 기간동안 트랜잭션의 상태가 _receipt_이 아니라면, _pending_ 상태라고 간주합니다. \(단위: ms\)

## Subscribing to a Transaction Status

새로 생성한 Henesis 인스턴스의 `subscribe`  함수를 이용하여 트랜잭션 상태를 구독하는 `subscription` 을 만들 수 있습니다.

```javascript
// const subscription = await henesis.subscribe(topic: String, options: Object)

const subscription = await henesis.subscribe(
  "transaction",
  {
    subscriptionId: "subscription-id",
    ackTimeout: 30 * 1000
  }
);
```

* `topic` : 구독할 토픽입니다. 트랜잭션 상태 변화를 듣기 위해서는 `transaction`을 입력합니다. 
* `options` : `subscription`생성에 필요한 정보.
  * `subscriptionId`: Henesis는 `subscription` 생성 시점부터 트랜잭션 상태를 유실없이 전달합니다. 네트워크 상황 등으로 인해 연결이 끊기더라도, 재연결이 된다면 마지막으로  전달이 성공된 데이터 이후부터 다시 전송합니다. `subscriptionId`는 유저가 지정할 수 있습니다. 만약 같은 `subscriptionId`가 존재하지 않는다면 새로운 `subscription`이 생성됩니다.
  * `ackTimeout`: Acknowledge를 기다리는 시간\(단위: ms\), `ackTimeout` 내에 Henesis SDK로 부터 ACK이 전달되지 않으면 해당 메세지가 제대로 전달되지 않았다고 판단하여 다시 전송합니다. 기본 값은 10000ms 이며 범위는 10000ms  ~ 600000ms\(10s  ~ 600s\) 입니다.

해당 `subscription` 인스턴스를 이용하여 다음과 같이 트랜잭션 상태를 추적할 수 있습니다.

```javascript
subscription.on("message", async (message) => {
	switch(message.result.type) {
		case 'pending' : {
			// trackTransaction 함수 호출시 설정한 timeout이 지났을  
			// TODO: Retry 로직을 작성하면 됩니다.
			break;
		}	
		case 'receipt' : {
			// transaction이 채굴될 
			break;
		}	
		case 'confirmation' : {
			// transaction이 채굴된 후 'confirmation'개의 블록이 추가 생성될 때
			break;
		}	
	}
	message.ack();// 반드시 호출해야 합니다. 
}

subscription.on("error", async (error) => {
	// handling errors	
});
```

Henesis에서 전달하는 데이터를 구독하기 위해서 `on` 함수를 사용합니다. `subscription` 에는 2가지 토픽이 존재하며, 각 토픽의 데이터는 아래와 같은 상황에서 발생합니다. 각 토픽에서 전달되는 데이터를 이용해 원하는 로직을 구현할 수 있습니다. 

| Topic | 발생 상황 |
| :--- | :--- |
| message | 추적하는 트랜잭션의 상태가 변경됬을 때 |
| error | Henesis에 장애 상황이 발생했을 때 \(네트워크 에러, 노드 에러\) |

### "message" 토픽으로 구독하는 데이터 

`message` 토픽을 통해 3가지 `type` 의 데이터를 구독할 수 있습니다. 

* **`pending`** `henesis.trackTransaction`을 호출하고 `timeout` 이 지난 후에도 채굴이 되지 않았을 때 발생합니다. 

```javascript
{
   'ackId':'400fcbf0-f8c2-488f-a582-311d90a89733',
   'id':'4770a7fa-f121-4a73-986f-e96e20101c22',
   'data':{
      'type':'pending',
      'result':{
         'transactionHash':'0xda21fb02bd65f8c67719186c7623680bb4fa4b80fd35da4cd86f20b8d0e86d3d',
         'confirmation':5,
         'timeout':5000
      }
   }
   ...
}
```

* **`receipt`**  추적하는 `txHash` 가 채굴 되었을 때 발생합니다.

```javascript
{
   "ackId":"44b18d27-26fb-42d2-b5cb-2ea7971f17e1",
   "id":"435330c7-7963-4a20-97a7-5f141a83d827",
   "data":{
      "type":"receipt",
      "result":{
         "blockHash":"0xf7c6fd4acbd2f8fff3e8967a43ef321e1a1b6434a697ea26549e98d4de803394",
         "blockNumber":6570550,
         "contractAddress":null,
         "cumulativeGasUsed":166177,
         "from":"0x66433dcda33f2cb569656ec5f0af203b6fc02964",
         "gasUsed":21004,
         "logs":[

         ],
         "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
         "root":null,
         "status":true,
         "to":"0x66433dcda33f2cb569656ec5f0af203b6fc02964",
         "transactionHash":"0x157cc935aaf2769815e08a4e337ea50fbf4340a31bae2e7618d6772f3bb66450",
         "transactionIndex":2
      }
   }
   ...
}
```

* **`confirmation`**  추적하는 `txHash` 가 채굴되고`confirmation` 수 만큼 추가 블록이 생성되었을 때 발생합니다. 

```javascript
{
   "ackId":"44b18d27-26fb-42d2-b5cb-2ea7971f17e1",
   "id":"435330c7-7963-4a20-97a7-5f141a83d827",
   "data":{
      "type":"confirmation",
      "result":{
         "blockHash":"0xf7c6fd4acbd2f8fff3e8967a43ef321e1a1b6434a697ea26549e98d4de803394",
         "blockNumber":6570550,
         "contractAddress":null,
         "cumulativeGasUsed":166177,
         "from":"0x66433dcda33f2cb569656ec5f0af203b6fc02964",
         "gasUsed":21004,
         "logs":[

         ],
         "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
         "root":null,
         "status":true,
         "to":"0x66433dcda33f2cb569656ec5f0af203b6fc02964",
         "transactionHash":"0x157cc935aaf2769815e08a4e337ea50fbf4340a31bae2e7618d6772f3bb66450",
         "transactionIndex":2
      }
   }
   ...
}
```

  
  


