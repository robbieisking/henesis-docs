# \[Tutorial\] Replacing Pending TXs

## Tx-Tracker <a id="tx-tracker"></a>

Tx Tracker의 사용법 및 활용법을 알아봅니다. 여러분은 이 튜토리얼을 통해 

* **Tx-Tracker의 사용법을 알 수 있습니다.**
* **Tx-Tracker를 사용해서 pending 상태의 transaction을 auto-resolving할 수 있습니다.** 

또한, 튜토리얼의 원활한 진행을 위해 대시보드를 제공합니다. 여러분은 이 대시보드를 통

* **트랜잭션을 생성하여 블록체인 네트워크에 전파할 수 있습니다.**
* **발생시킨 트랜잭션의 상태를 대시보드에서 확인할 수 있습니다.** 

![](../.gitbook/assets/2019-11-08-9.22.18.png)

{% hint style="warning" %}
튜토리얼에서는 pending 상태의 transaction이 auto-resolve가 되는 과정을 보이기 위해 Tx-Tracker에서는 제공하지 않는 `replaced`라는 status가 사용됩니다. transaction이 pending이 되는 경우는 크게 [두 가지](https://docs.henesis.io/v/ko/tracking-transactions/introduction#undefined-1) 경우가 있습니다. 이 때, 오랫동안 pending이 되는 상태를 막고자 pending상태인 transaction의 nonce와 같은 nonce, 더 높은 gas price로 새로운 transaction을 생성하며 채굴 경쟁에서 밀린 transaction의 상태 `replaced`로 정의합니다.
{% endhint %}

## Project Structure <a id="project-structure"></a>

{% tabs %}
{% tab title="./sample-tx-tracker" %}
```text
sample-tx-tracker/
├── /config          
├── /helper  # helper library for make transaction         
├── /public          
├── /src     # frontend code
├── /types   # types used in index.js
├── /store   # transaction store used in index.js
├── .env     # configuration file
├── index.js # API server code
├── jsconfig.json      
├── package.json
...
```
{% endtab %}
{% endtabs %}

* API 서버는 다음과 같은 인터페이스를 가지고 있습니다.
  * **POST** `/api/tx`: 트랜잭션을 생성하고 블록체인 네트워크에 전파합니다.
  * **GET** `/api/tx` : 네크워크에 전파한 모든 트랜잭션의 상태값을 반환합니다. 
* Frontend
  * API 서버의 **GET** `/api/tx` 를 지속적으로 폴링하여 트랜잭션의 상태를 갱신합니다.
  * 유저가 `Generate Transaction` 버튼을 누르면, **POST** `/api/tx` 요청을 API서버에 전송합니다.

## Step By Step <a id="step-by-step"></a>

1. Sample repository 가져오기

   ```bash
   git clone https://github.com/HAECHI-LABS/sample-tx-tracker
   ```

2. 의존성 설치하기

   ```bash
   npm install
   ```

3. `.env` 변경하기

   * `CLIENT_ID` : `henesis account:describe` 를 통해 확인할 수 있습니다.
   * `PRIVATE_KEY` :  테스트를 위해 사용할 지갑의 프라이빗 키입니다. **충분한 양의 이더리움이 있는 EOA 주소의 프라이빗 키를 입력합니다.**
   * `NODE_ENDPOINT` : 연결할 블록체인 노드 endpoint 입니다.
   * `PLATFORM`: 원하는 플랫폼을 선택합니다. 지원 가능한 플랫폼 및 네트워크는 [여기](https://docs.henesis.io/v/ko/faq/supported-blockchains)에서 확인가능합니다.
   * `NETWORK`: 원하는 네트워크를 선택합니다.

   ```javascript
   CLIENT_ID=<your-client-id>
   PRIVATE_KEY=<your private key>
   NODE_ENDPOINT=https://ropsten.infura.io/v3/<your-key>
   PLATFORM=ethereum
   NETWORK=ropsten
   ```

4. 소스코드를 빌드하기

   ```bash
   npm run build:standalone
   ```

5. 서버를 실행하고 브라우저에[ http://localhost:3000](http://localhost:3000) 탐색하기

   ```bash
   node index.js
   ```

## 작동원리 <a id="how-does-it-works"></a>

#### Transaction tracker using Henesis SDK <a id="tracking-transaction-using-henesis-sdk"></a>

`index.js` 가 이번 튜토리얼에서 가장 중요한 부분입니다. 이곳에서 Henesis SDK를 활용하여 트랜잭션 상태를 추적합니다. 

`CLIENT_ID` 를 이용하여 Henesis Server와의 인증을 진행하고 원하는 `platform`과 `network`를 설정하여 데이터 구독을 위한 Henesis 인스턴스를 생성할 수 있습니다\([참고](interacting-the-transaction-tracker.md#authentication)\).   

{% tabs %}
{% tab title="index.js" %}
```javascript
const {CLIENT_ID, PRIVATE_KEY, NODE_ENDPOINT, PLATFORM, NETWORK} = process.env;
const tracker = new TransactionTracker(CLIENT_ID, {
  platform: PLATFORM,
  network: NETWORK
});
```
{% endtab %}
{% endtabs %}

API 서버가 `henesis#trackTransaction` 를 이용하여 트랜잭션을 추적합니다\([참고](interacting-the-transaction-tracker.md#tracking-transactions)\). 

* 트랜잭션을 발생시키고 `transactionHash` 를 얻습니다.
* `transactionHash` 를 `henesis#trackTransaction` 에 넣어 추적을 시작합니다.
* `timeout`과`confirmation`을 설정합니다.

{% tabs %}
{% tab title="index.js" %}
```javascript
app.post('/api/tx', async function (req, res) {
  //Generate Transactions
  const nonce = await sender.getNonce();
  const transactionHash = await sender.send(nonce, GAS_PRICE);
  console.log(`transaction generated. txHash:${transactionHash}`);

  //start tracking transaction
  await tracker.trackTransaction(transactionHash, {
    timeout: TIMEOUT,
    confirmation: CONFIRMATION
  });

  const transaction = new Transaction(
    transactionHash,
    nonce,
    GAS_PRICE
  );
  transactionStore.save(transaction);
  await res.json(transaction);
});
```
{% endtab %}
{% endtabs %}

`subscription` 구독을 통해 tracking하고 있는 transaction의 상태 및 정보를 받아볼 수 있습니다.

* `message.data.type` 에서 추적한 트랜잭션의 상태를 알 수 있습니다.
* `message.data.result` 에서 추적한 트랜잭션의 정보를 알 수 있습니다.
* `message.ack()` 를 마지막에 반드시 호출하여야 합니다. 

Tx-Tracker에서 tracking하는 status는 `pending`, `receipt`, `confirmation` 세 가지입니다. 만약 `pending` 상태라면 transaction을 resolve하기 위한 로직을 수행합니다. 이 전 transaction들의 nonce를 확인하여 resolve가 필요한 transaction일 경우에 resolve를 진행합니다.

{% hint style="warning" %}
현재 Tx-Tracker는 pending 상태의 transaction일 때, transaction data를 제공하지 않습니다. 그렇기 때문에 auto-resolving을 위해 TransactionStore를 사용합니다.
{% endhint %}

transaction이 채굴되면, `receipt` 상태가 됩니다. `checkResolvedTransaction` 함수를 통해 만약 resolve가 된 transaction이라면 채굴경쟁에서 밀린 transaction의 상태를 `replaced` 로 바꿉니다. 

{% tabs %}
{% tab title="index.js" %}
```javascript
async function trackTx() {
  const subscription = await tracker.subscribe(
    "transaction",
    {
      subscriptionId: "your-subscription-id",
      ackTimeout: 30 * 1000 // default is 10 * 1000 (ms)
    }
  );

  subscription.on("message", async (message) => {
    const transactionHash = message.data.result.transactionHash;
    let transaction = {};
    console.log(`[MESSAGE] transaction ${transactionHash} status is: ${message.data.type}`)
    switch (message.data.type) {
      case 'pending' :
        transaction = transactionStore.findByHash(transactionHash);
        if (transaction.status == undefined) {
          transaction.status = Status.pending;
        }
        if (isNeededResolve(transaction)) {
          const newTransaction = await retry(transaction);
          transactionStore.save(newTransaction);
        }
        break;
      case 'receipt' :
        transaction = transactionStore.findByHash(transactionHash);
        checkResolvedTransaction(transaction);
        transaction.status = Status.receipt;
        transaction.data = {...message.data.result};
        transactionStore.save(transaction);
        break;
      case 'confirmation' :
        transaction = transactionStore.findByHash(transactionHash);
        transaction.status = Status.confirmation;
        transaction.data = {...message.data.result};
        transactionStore.save(transaction);
        break;
    }
    message.ack();
  });

  subscription.on("error", async (error) => {
    console.log('err', error);
  });
}
```
{% endtab %}
{% endtabs %}

