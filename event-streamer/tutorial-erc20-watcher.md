# \[Tutorial\] ERC20 Watcher

## ERC20 Watcher‌ <a id="erc20-watcher"></a>

ERC20 Watcher는 이벤트를 구독할 수 있는 간단한 대시보드입니다. 대시보드를 통해 Tether ERC20 토큰의 이벤트를 대시보드에 표현할 수 있습니다.

![](../.gitbook/assets/2019-10-16-11.09.49.png)

## Project Structure

{% tabs %}
{% tab title="./sample-erc20-watcher" %}
```text
sample-erc20-watcher/
├── /config          
├── /contracts # ERC20 contract files
├── /public          
├── /src # frontend code
├── .env # configuration file
├── henesis.yaml # configuration file for integration         
├── index.js   # API server code
├── jsconfig.json   
├── package.json    
...
```
{% endtab %}
{% endtabs %}

* API 서버는 아래와 같은 인터페이스로 이뤄져있습니다.
  * **GET** `/api/events` : 모든 이벤트 정보를 반환합니다.
* Frontend
  * API 서버의 **GET** `/api/events` 엔드포인트를 지속적으로 폴링하여 이벤트 정보를 구독합니다.

## Step by Step

1. [Henesis CLI](../installation/henesis-cli.md) 설치
2. Sample repository 가져오기

   ```text
   git clone https://github.com/HAECHI-LABS/sample-erc20-watcher
   ```

3. dependency 설치

   ```text
   npm install
   ```

4. 로그인\(아직 하지 않았다면\)

   ```text
   henesis login
   ```

5. Integration 배포

   ```text
   henesis integration:deploy
   ```

6. Integration Id와 Client Id 확인
   * Integration Id: `henesis integration:status`
   * Client Id: `henesis account:describe`npm ru
7. `.env` 에 `CLIENT_ID`  `INTEGRATION_ID` 변경

   ```javascript
   CLIENT_ID=<your client id>
   INTEGRATION_ID=<your integration id>
   ```

8. 소스 코드 빌드

   ```
   npm run build:standalone
   ```

9. API 서버를 실행하고 브라우저에서 [http://locahost:3000](http://locahost:3000) 탐색

   ```
   node index
   ```

## 작동원리

이 튜토리얼에서는 두 가지 중요한 부분이 있습니다.  

#### Integration 배포 

Step 5에 `henesis integration:deploy` 명령어를 입력하면 `henesis.yaml` 에 있는 설정 파일대로  Integration이 배포 됩니다. 따라서 `henesis.yaml` 을 적절하게 작성하는 것이 중요합니다\([참고](subscribing-events-via-websocket.md#henesis-yaml)\).

{% tabs %}
{% tab title="henesis.yaml" %}
```yaml
version: v1
name: tether-tuto 

filters:
  contracts:
  - address: '0xdac17f958d2ee523a2206206994597c13d831ec7'
    path: ./contracts/TetherToken.sol
    name: TetherToken 
    compilerVersion: 0.4.18

blockchain:
  platform: ethereum 
  network: mainnet 
  threshold: 5
  interval: 1000

provider:
  type: webSocket
```
{% endtab %}
{% endtabs %}

#### Subscribing Event with Henesis SDK

이번 튜토리얼에서 중요한 또 다른 부분은 `index.js` 입니다. 이곳에서 Henesis SDK를 이용하여 이벤트를 구독합니다. 자세히 살펴봅시다. 

* Client Id를 이용하여 데이터 구독을 위한 Henesis 인스턴스를 생성할 수 있습니다.

{% tabs %}
{% tab title="index.js" %}
```typescript
import { EventStreamer } from '@haechi-labs/henesis-sdk-js'
...
async function henesis() {
  // create henesis instance
  const eventStreamer = new EventStreamer(CLIENT_ID);
}
```
{% endtab %}
{% endtabs %}

* 새로 생성한 인스턴스의 `henesis#subscribe` 를 이용하여 subscription 을 만들 수 있습니다.
  *  `subscriptionId` 은 유일한 값이야 한다는 사실을 잊지 마세요\([참고](subscribing-events-via-websocket.md#subscription)\).

{% tabs %}
{% tab title="index.js" %}
```typescript
import { EventStreamer } from '@haechi-labs/henesis-sdk-js'

async function henesis( ) {
  ...
  // subscribe "streamedBlock", then create subscription object.
  const subscription = await eventStreamer.subscribe(
    "streamedBlock",
    {
      integrationId:INTEGRATION_ID,
      subscriptionId: "your-subscription-id"
      ackTimeout: 30 * 1000 // optional. (default: 10000, unit: ms)
    }
  );
}

```
{% endtab %}
{% endtabs %}

* 다음과 같은 코드를 통해 event를 듣고 파싱하여 원하는 작업을 할 수 있습니다\([참조](subscribing-events-via-websocket.md#subscription)\).

{% tabs %}
{% tab title="index.js" %}
```javascript
import Henesis from '@haechi-labs/henesis-sdk-js'

async function henesis( ) {
  ...
  subscription.on('message', async message => {
    // In case of disconnection due to network abnormalities (such as Wi-Fi problem), up to one duplicated message can be delivered.
    // You can check message duplication with messageId or block number.
    if (getBlockNumber(message) > processedBlockNumber) {
      const events = messageToEvents(message);
      // processing events
      // For example, you can save events to your database.
      events.forEach(event => model.push(event));
      //console.log(JSON.stringify(events, undefined, 2));
      console.log(`data received, event:${events}`);
      // You need to remember the processed index(messageId or block number) of the message you received.
      setProcessedBlockNumber(message);
    }
    message.ack(); // (MUST) Send an ACK message even if duplicated message!!
  });
  
  subscription.on('error', err => {
    console.error(err);
  });

  subscription.on('close', err => {
    console.error(err);
  });
  ...
}
```
{% endtab %}
{% endtabs %}

