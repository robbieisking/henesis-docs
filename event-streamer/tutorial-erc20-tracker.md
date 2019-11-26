# \[チュートリアル\] ERC20 Token Watcher

## ERC20 Token Watcher‌ <a id="erc20-watcher"></a>

ERC20 Token Watcherは、ERC20 コントラクトからイベントを取得することができる簡単なダッシュボードです。ダッシュボードを使用して[Tether ERC20](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7)トークンのイベントをダッシュボードに表現することができます。

![](../.gitbook/assets/2019-10-16-11.09.49.png)

## プロジェクトの構造

{% code title="./sample-erc20-watcher" %}
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
{% endcode %}

* APIサーバーは、以下のようなインターフェースで構成されています。
  * **GET** `/api/events` : すべてのイベント情報を返します。
* Frontend
  * APIサーバーの**GET** `/api/events` エンドポイントを継続的に調べ、イベント情報を取得します。

## ステップ

1. [Henesis CLI](../installation/henesis-cli.md)のインストール
2. Sample repositoryインポート



   ```bash
   git clone https://github.com/HAECHI-LABS/sample-erc20-watcher
   ```

3. 依存関係にあるライブラリのインストール

   ```bash
   npm install
   ```

4. ログイン（まだしていなかった場合）

   ```bash
   henesis login
   ```

5. Integrationをデプロイ



   ```bash
   henesis integration:deploy
   ```

6. Integration IdとClient Idの確認
   * integration-Id: `henesis integration:status`
   * client-Id: `henesis account:describe`
7. `.env`の`CLIENT_ID` と`INTEGRATION_ID`を自分の情報に変更する

{% code title=".env" %}
```javascript
CLIENT_ID=<your clien id>
INTEGRATION_ID=<your integration id>
```
{% endcode %}

1. ソースコードをビルド

   ```bash
   npm run build:standalone
   ```

2. APIサーバーを実行して、ブラウザで[http://locahost:3000](http://locahost:3000)をナビゲーション

```bash
node index.js
```

## 動作原理

このチュートリアルでは重要なポイントが2点あります。

#### 1. Integrationの配布

上述したステップ5の`henesis integration：deploy`コマンドを入力すると、`henesis.yaml`の設定ファイルに従ってIntegrationが配布されます。したがって`henesis.yaml`を適切に作成することが重要です（[参照](https://docs.henesis.io/v/ko/subscribing-events/subscribing-events-via-websocket#henesis-yaml)）。

{% code title="henesis.yaml" %}
```yaml
version: v1
name: tether-tutorial

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

provider:
  type: webSocket
```
{% endcode %}

#### 2.  Henesis SDKを使ってデータを取得する

重要なポイントのもう一つは`index.js`です。このファイルでHenesis SDKを利用してイベントを取得します。詳しく見てみましょう。 

* `Client Id`を利用して、データのサブスクリプションのためのHenesisインスタンスを生成することができます。

{% code title="index.js" %}
```typescript
import Henesis from '@haechi-labs/henesis-sdk-js'
...
async function henesis() {
  // create a new Henesis instance
  const henesis = new Henesis(CLIENT_ID);
}
```
{% endcode %}

*  `subscribe` メソッドを使ってwebSocketを開始するのは簡単ですが、 `subscriptionId` は1つの取得で一意であることには注意が必要です。（[参照](https://docs.henesis.io/v/ko/subscribing-events/subscribing-events-via-websocket#subscription)）
* 新しく作成したインスタンスの`henesis＃subscribe`を利用して、subscriptionを作成することができます。 

{% code title="index.js" %}
```typescript
import Henesis from '@haechi-labs/henesis-sdk-js'

async function henesis( ) {
  ...
  const subscription = await henesis.subscribe(
    "streamedBlock",
    {
      integrationId:INTEGRATION_ID,
      subscriptionId: "your-subscription-id"
    }
  );
}

```
{% endcode %}

* 次のコードを使用してイベントを取得し解析することができます（[参照](https://docs.henesis.io/v/ko/subscribing-events/subscribing-events-via-websocket#subscription)）。

{% code title="index.js" %}
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
{% endcode %}

