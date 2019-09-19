# Henesis SDK 연동하기

이후,  `src/event.js`의 함수에 다음의 코드를 추가하여 `henesis` 객체를 생성해 줍니다.

```typescript
import Henesis from '@haechi-labs/henesis-sdk-js'

export default async function henesisMain({ config, model }) {
  // create henesis instance
  const henesis = new Henesis();
}
```

이후, WebSocket 구독을 위한 subscribe 함수를 실행합니다.

우선 사용자가 Henesis와 통신하기 위해서 Henesis SDK를 설치해야합니다. Henesis SDK는 사용자가 Henesis로부터 WebSocket을 통해서 데이터를 받아오는 것을 도와줍니다. 

자세한 Henesis SDK 사용법은 [이곳](https://github.com/HAECHI-LABS/henesis-sdk-js)에서 확인해 보실 수 있습니다.

다음의 명령어를 통해 라이브러리를 프로젝트에 추가합니다.

```bash
cd ./backend
npm i -S @haechi-labs/henesis-sdk-js
```

```typescript
import Henesis from '@haechi-labs/henesis-sdk-js'

export default async function henesisMain({ config, model }) {
  ...
  
  // subscribe "streamedBlock", then create subscription object.
  const subscription = await henesis.subscribe(
    config.integrationId,
    "streamedBlock",
  );
}
```

`henesis.subscribe(config.integrationId, "streamedBlock")` 을 통해 `subscription` 객체를 생성하고 이를 이용하여 데이터를 가져옵니다. 

```javascript
const subscription = await henesis.subscribe(
    config.integrationId,
    "streamedBlock",
    {
        subscriptionId: "mySubId"
    }
)
```

Henesis의 WebSocket에서는 각 subscription 마다 id가 할당되며, 사용자가 지정해주지 않는다면 uuid로 생성됩니다. 만약, 특정한 subscriptionId를 지정하고 싶은 경우 아래 코드처럼 인자를 변경해 줄 수 있습니다. 

기본적으로, Henesis는 구독 시점 이후의 블록들로부터 발생한 이벤트들만 전달해주지만, 이전에 사용했던 subcriptionId를 재사용할 경우, 그 사이 발생했던 이벤트들을 모두 전달합니다.  
이는 예상치 못한 접속 장애들이 발생하였을 경우에도 데이터들을 빠짐없이 전달하기 위함입니다.

{% hint style="danger" %}
한 개의 subscription ID로 연결 가능 클라이언트의 수는 1개입니다. 따라서 여러개의 클라이언트가 동일한 subscription ID로는 연결 할 수 없습니다.
{% endhint %}

마지막으로 Henesis SDK의 WebSocket으로 데이터를 전달 받은 후 API 서버 메모리에 다음과 같이 저장합니다.

### Event Schema

Henesis에서 제공해주는 event는 아래와 같은 형태로 제공됩니다

```javascript
{
    "events": [
        {
            "contractAddress":"0x06012c8cf97bead5deae237070f9587f8e7a266d",
            "contractName":"KittyCore",
            "eventName":"Birth(address,uint256,uint256,uint256,uint256)",
            "data": [ // event의 argument값이 표시됩니다.
              {
                "name": "owner",
                "value": "0x0aCFD42cE979D636386AAE2b536d99A06f138445",
                "type": "address"
              },
              {
                "name": "kittyId",
                "value": "1702702",
                "type": "uint256"
              },
              {
                "name": "matronId",
                "value": "1702587",
                "type": "uint256"
              },
              {
                "name": "sireId",
                "value": "1701172",
                "type": "uint256"
              },
              {
                "name": "genes",
                "value": "242173103761036926721863388541588786602640453556422932120028984341149153",
                "type": "uint256"
              }
            ],
            "transaction": // transaction 관련된 정보(hash 등)
        } ],
    "blockMeta":{  }, // block과 관련된 정보 (platform, network, blockNumber,header등)
    "userMeta":{  } // 사용자 정보
}
```

Henesis SDK에서는 `message.data`에서 위와 같은 정보를 받아볼 수 있습니다. 따라서 아래와 같이 데이터를 파싱한 후 저장합니다.

```typescript
import Henesis from '@haechi-labs/henesis-sdk-js'

export default async function henesisMain({ config, model }) {
  ...
  
  // when receive message from Henesis save event
  // to memory, then send ack message to Henesis
  subscription.on('message', async (message) => {
    const event = messageToEvent(message);
    
    // save event to in-memory db
    model.save(event)
    
    message.ack();
  });

  subscription.on('error', err => {
    console.error(err);
  });

  // messageToEvent parse message and convert it to event
  function messageToEvent(message) {
    const event = message.data.events[0];
    const blockMeta = message.data.blockMeta
    return {
      event: event.eventName.split('(')[0],
      contract: event.contractName,
      transactionHash: event.transaction.hash,
      args: dataToArgs(event.data),
      blockMeta
    }
    
    function dataToArgs(data) {
      const res = {}
      for (let item of data) {
        res[item.name] = item.value
      }
      return res
    }
  }
}
```

`subscription.on('message', message => {...})` 를 통해서 현재 듣고 있는 스마트 컨트랙트로부터 이벤트가 발생했을 때 해당 이벤트와 관련된 데이터가 넘어옵니다. 그리고 이벤트들을 도메인에 맞게 변형한 후 저장한 뒤, `message.ack()`을 통해서 Henesis에 응답합니다.

{% hint style="warning" %}
성공적으로 데이터를 수신하였을 경우 message의 ack 함수를 반드시 실행시켜 주어야합니다. 해당 함수 실행이 없다면 Henesis는 message가 전송되지 않은 것으로 판단합니다.
{% endhint %}

완성된 코드는 다음과 같습니다.

```javascript
import Henesis from '@haechi-labs/henesis-sdk-js'

export default async function ({config, model}) {

    const henesis = new Henesis();
    const subscription = await henesis.subscribe(
        config.integrationId,
        "streamedBlock"
    )
    subscription.on('message', async (message) => {
        const event = messageToEvent(message)
        model.save(event)
        console.log(`data received, event:${event.event}`)
        message.ack();
    });

    subscription.on('error', err => {
        console.error(err);
    });

    //parsing logic
    function messageToEvent(message) {
        const event = message.data.events[0];
        const blockMeta = message.data.blockMeta
        return {
            event: event.eventName.split('(')[0],
            contract: event.contractName,
            transactionHash: event.transaction.hash,
            args: dataToArgs(event.data),
            blockMeta
        }
        function dataToArgs(data) {
            const res = {}
            for (let item of data) {
                res[item.name] = item.value
            }
            return res
        }
    }
}
```

### Integration Id 입력하기

henesis cli를 이용하여 integration id를 조회합니다

```text
henesis integration:status
Id                          Name                  Platform     Network   Version   Provider     State          
sample-vokpc                sample                ethereum     mainnet    v1        webSocket    Available      

```

backend/src/config.json을 수정합니다.

{% code-tabs %}
{% code-tabs-item title="backend/src/config.json" %}
```text
{
	"port": 8080,
	"corsHeaders": ["Link"],
	"integrationId": "sample-vokpc"
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

