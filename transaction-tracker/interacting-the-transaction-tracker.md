---
description: Learn how to track transactions using the Henesis SDK.
---

# Interacting with the Transaction Tracker

## Henesis SDK

For more information about installing the SDK, [click here](../installation/henesis-sdk.md).

You can track the status of transactions through the Henesis SDK. The SDK currently supports the javascript and will support other languages soon.

## Authentication

To use the Henesis SDK, you first need a `client-id`. The `client-id` is the unique information assigned to each user and you can check this information by using the Henesis CLI as shown below.

```bash
$ henesis account:describe

Email: haechi@haechi.io
Name: haechi
Organization: haechi-labs
clientId: <client-id>
```

You can create a Henesis instance for data subscription with `client-id` as below.

```javascript
import {TransactionTracker} from '@haechi-labs/henesis-sdk-js'

const transactionTracker = new TransactionTracker('client-id', {
    // platform and network options
});
```



## Select Platform and Network

After authenticating with `client-id`, select the `platform` and `network` you want to use.

```typescript
import {TransactionTracker} from '@haechi-labs/henesis-sdk-js'

const transactionTracker = new TransactionTracker('client-id', {
    platform: 'ethereum',
    network: 'ropsten'
});
```

The `platform` and`network` supported in Henesis can be found [here](https://docs.henesis.io/faq/supported-blockchains)

## Tracking Transactions

To track the status of a transaction, use the `txHash` generated when you send a transaction. By using the `trackTransaction` method provided by the SDK, you can track a status of the transaction.

```javascript
const txHash = await web3.eth.sendRawTransaction(sign(privateKey, txData))

henesis.trackTransaction(txHash, {
	"confirmation": 12,
	"timeout": 30 * 1000,
});
```

* `confirmation`: To confirm the transaction as `confirmation` status, you can set how many more blocks must be created after the block containing the tracked transaction. In general, `12` \(block confirmation\) is safe enough for preventing a chain reorganization.
* `timeout`:  How long to wait for the transaction to be mined. If the transaction is not mined within `timeout` , the transaction state will become `pending`state.  \(unit: ms\)

## Subscribing to a Transaction Status

You can create a `subscription` by calling `subscribe` method of the newly created instance.

```javascript
// const subscription = await henesis.subscribe(topic: String, options: Object)

const subscription = await henesis.subscribe(
  "transaction",
  {
    subscriptionId: "subscription-id",
    ackTimeout: 30 * 1000 // optional. (default: 10000, unit: ms)
  }
);
```

* `topic` : Topic for subscribing to. Enter `transaction` to listen to changes of transaction status.
* `options` : Information needed to create a subscription.
  * `subscriptionId`: Henesis delivers blockchain data without loss from the time of `subscription` creation. Even if the connection is lost due to network conditions, when the network is reconnected, the data is transferred after the last successful delivery. You can specify `subscriptionId`. If the same `subscriptionId` does not exist, you can create new one.
  * `ackTimeout`: Time for waiting acknowledge message \(unit: ms\). If the ACK is not delivered from the Henesis SDK within the `ackTimeout`, it is determined that the message was not delivered correctly. Then, Henesis re-sends messages. The default value is 10000ms and the range is 10000ms to 600000ms \(10s to 600s\).

Messages could be received through the `subscription#on()`.

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

To subscribe to data from Henesis, use `on` function. There are three topics on `subscription#on`. The data for each topic is triggered as below. You can implement logic by using the data from each topic.

| Topic | Trigger |
| :--- | :--- |
| message | When a transaction status is changed. |
| error | When an error occurred at the Henesis \(e.g. network, node failure \). |

### Message Format

The `message` topic allows you to subscribe to **three types of data.**

* **`pending`** It occurs when a transaction has not been mined after calling henesis.trackTransaction and after a timeout.

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

* **`receipt`**  It occurs when the transaction is mined.

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

* **`confirmation`**  This happens when the number of 'confirmation\` blocks created, after the transaction is mined.

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

  
  


