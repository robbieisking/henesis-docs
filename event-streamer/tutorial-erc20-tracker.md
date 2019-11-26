# \[Tutorial\] ERC20 Token Watcher

## ERC20 Token Watcher‌ <a id="erc20-watcher"></a>

The ERC20 token watcher is a simple dashboard for subscribing events from a ERC20 smart contract. You are able to‌ show events of the [Tether ERC20 smart contract](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7) on the dashboard.

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

* The API server has the following interface.
  * **GET** `/api/events` : Returns all events information.
* Frontend
  * keep polling the end point, **GET** `/api/events` , to subscribe to all events information.

## Step by Step

1. Install the [Henesis CLI](../installation/henesis-cli.md)
2. Clone the sample repository

   ```
   git clone https://github.com/HAECHI-LABS/sample-erc20-watcher
   ```

3. Install dependencies

   ```bash
   npm install
   ```

4. Login \(If you did not login yet\)

   ```bash
   henesis login
   ```

5. Deploy Integration

   ```
   henesis integration:deploy
   ```

6. Check the integration-Id and client-Id
   * integration-Id: `henesis integration:status`
   * client-Id: `henesis account:describe`
7. Modify the `CLIENT_ID` and `INTEGRATION_ID` in the `.env`

   ```javascript
   CLIENT_ID=<your clien id>
   INTEGRATION_ID=<your integration id>
   ```

8. Build source codes.

   ```bash
   npm run build:standalone
   ```

9. Run the API server and browse [http://locahost:3000](http://locahost:3000)

   ```
   node index.js
   ```

## How does it works?

There are two main parts in this tutorial: Deployment integration and subscription to events through the Henesis SDK.

#### Deploy Integration

When you type `henesis integration:deploy` command at step 5, the Henesis CLI reads `henesis.yaml` and deploys an integration based on this configuration. Therefore, it is important to correctly set the [`henesis.yaml` file](deploy-integration.md#henesis-yaml).

{% tabs %}
{% tab title="henesis.yaml" %}
```yaml
version: v1
name: tether-tutorial

filters:
  contracts:
    - address: '0xdac17f958d2ee523a2206206994597c13d831ec7'
      name: TetherToken
      files: # The events of the contracts listed below can be combined together at this address.
        - path: ./contracts/TetherToken.sol
          contractName: TetherToken
          compilerVersion: 0.4.18

blockchain:
  platform: ethereum 
  network: mainnet 
  threshold: 5

provider:
  type: webSocket
```
{% endtab %}
{% endtabs %}

#### Subscription to Events through the Henesis SDK

`index.js` is also important in this tutorial as well. This subscribes to events by using the Henesis SDK.

* You can make a new Henesis instance for [subscribing to data](subscribing-events-via-websocket.md#subscription) by using the `CLIENT_ID` .

{% tabs %}
{% tab title="index.js" %}
```typescript
import { EventStreamer } from '@haechi-labs/henesis-sdk-js'
...
async function henesis() {
  // create a new Henesis instance
  const eventStreamer = new EventStreamer(CLIENT_ID);
}
```
{% endtab %}
{% endtabs %}

* It is easy to initiate a webSocket subscription by using the `subscribe` method.‌ Don't forget the `subscriptionId` should be unique per subscription.

{% tabs %}
{% tab title="index.js" %}
```typescript
import { EventStreamer } from '@haechi-labs/henesis-sdk-js'

async function henesis( ) {
  ...
  const subscription = await eventStreamer.subscribe(
    "streamedBlock",
    {
      integrationId:INTEGRATION_ID,
      subscriptionId: "your-subscription-id",
      ackTimeout: 30 * 1000 // optional. (default: 10000, unit: ms)
    }
  );
}

```
{% endtab %}
{% endtabs %}

* And you can subscribe to events and parse the data through the following codes.

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

