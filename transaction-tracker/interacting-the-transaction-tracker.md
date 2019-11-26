---
description: Henesisを利用してSDKのみでどのように簡単にトランザクションの状態を追跡することができるかどうか、実際に触れながら学んでいきましょう
---

# Transaction Trackerに触れてみよう

## Henesis SDK

Henesis SDKを使用して、トランザクションの状態を追跡することができます。現在SDKでは、javascript言語をサポートしていますが、将来的にはサポート言語を拡張する予定です。

インストールする方法は、[リンク](https://docs.henesis.io/v/ko/installation/henesis-sdk)を参照してください。

## 認証

Henesis SDKを利用するためには、`client-id`が必要です。 `client-id`は、アカウントごとに一意に付与された情報であり、以下のようにHenesis CLIを利用して確認することができます。

```bash
$ henesis account:describe

Email: haechi@haechi.io
Name: haechi
Organization: haechi-labs
clientId: <client-id>
```

SDKでは以下のように`client-id`を使用して、トランザクションの状態を追跡するためのインスタンスを生成することができます。

```javascript
import Henesis from '@haechi-labs/henesis-sdk-js'

const henesis = new Henesis('client-id');
```

## トランザクションの追跡

トランザクションの状態を追跡するためには、トランザクションを発生させるときに生成された`txHash`を使用します。生成されたtxHashを元にSDKで提供されている`tackTransaction`メソッドを用いる事でトランザクションを追跡するようにします。

```javascript
const txHash = await web3.eth.sendRawTransaction(sign(privateKey, txData))

henesis.trackTransaction(txHash, {
	"confirmation": 12,
	"timeout": 30 * 1000,
});
```

* `confirmation`: トランザクションが入れられたブロックの後に、何個のブロックが生成されれば`confirmation`状態と認めるか設定することができます。一般的に、`12`以上にするとchain reorganizationが起こる確率はほとんどありません。
* `timeout`:  トランザクションの状態がreceiptに変わるまで待機するタイムアウト時間を指定します。その期間内にトランザクションの状態が_receipt_に変化しなかった場合は、`pending`状態とみなされます。 （単位：ミリ秒）

## トランザクション状態の取得

新しく作成したHenesisインスタンスの`subscribe`メソッドを利用して、トランザクションの状態を取得する`subscription`を作成することができます。

```javascript
// const subscription = await henesis.subscribe(topic: String, options: Object)

const subscription = await henesis.subscribe(
  "transaction",
  {
    subscriptionId: "subscription-id"
  }
);
```

* `topic` : 取得するトピックです。トランザクションの状態の変化を取得するためには`transaction`を入力します。
* `options` : subscription生成に必要な情報です。
  * `subscriptionId`: Henesisは`subscription`生成時点からのトランザクションの状態をもれなく転送します。ネットワークの状況などにより接続が切断されても、再接続時に最後に転送成功したデータの後から再送信します。 `subscriptionId`はユーザが指定することができます。もし同じ`subscriptionId`が存在しない場合、新しい`subscription`が生成されます。

この`subscription`インスタンスを使用して、次のようにトランザクションの状態を追跡することができます。

```javascript
subscription.on('message', message => {
  switch (message.data.type) {
    case 'pending' : {
      // When a transaction is not mined within 'timeout', after the 'trackTransaction' function is called.
      // TODO: You can write a retry logic.
      break;
    }
    case 'receipt' : {
      // When a transaction is mined.
      break;
    }
    case 'confirmation' : {
      // When the number of 'confirmation` blocks created, after the transaction is mined.
      break;
    }
  }
  message.ack(); // must be called.
});

subscription.on('error', error => {
  // handling errors
  console.log(error);
});

```

Henesisから渡されるデータをサブスクライブするためには`on`関数を使用します。 `subscription＃on（）`には、2つのトピックが存在します。各トピックのデータは、以下のような状況で発生します。そして、各トピックで配信されるデータを利用して必要なロジックを実装することができます。

| Topic | Trigger |
| :--- | :--- |
| message | 追跡するトランザクションの状態が変更されたとき |
| error | Henesisに障害状況が発生したとき（ネットワークエラー、ノードエラー） |

### Messageのトピックにサブスクライブするデータ

`message`トピックを介して3つのタイプのデータを取得することができます。

* **`pending`** `henesis.trackTransaction`を呼び出してtimeoutが経過してもマイニングされていない場合に発生します。

```javascript
{
   "ackId":"400fcbf0-f8c2-488f-a582-311d90a89733",
   "id":"4770a7fa-f121-4a73-986f-e96e20101c22",
   "data":{
      "type":"pending",
      "result":{
         "transactionHash":"0xda21fb02bd65f8c67719186c7623680bb4fa4b80fd35da4cd86f20b8d0e86d3d",
         "confirmation":5,
         "timeout":5000
      }
   }
   ...
}
```

* **`receipt`**  追跡するtx​​Hashがマイニングされたときに発生します。

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

* **`confirmation`**  追跡するtx​​Hashがマイニングされてconfirmationの数だけ追加ブロックが生成されたときに発生します。

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

  
  


