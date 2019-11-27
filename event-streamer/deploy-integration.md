---
description: Learn what is Integration and how to deploy it.
---

# Deploy Integration

## Integration

Integration is a container unit where Henesis listens to events on the blockchain and delivers information to users. 

To deploy integration, the user must specify the following information:

* Smart contract information you would like to subscribe to \(e.g. address, files\).
* Information about blockchain platform and network \(e.g. Ethereum mainnet/testnet\).
* How to get events \(e.g. Webhook, WebSocket\).

## Prerequisite

* [Henesis CLI](../installation/henesis-cli.md)
* Smart contracts deployed on the blockchain.

{% hint style="info" %}
This section assumes that you already have the Henesis CLI and have smart contracts deployed on blockchain.   
You can check [the instruction](../installation/henesis-cli.md) on how to install the CLI.
{% endhint %}

If you have developed smart contracts using Truffle, you have a directory with following structure:

```bash
sample/
├── migrations
├── contracts
    └── example.sol
├── tests
├── truffle-config.js
```

Let's see how to write a `henesis.yaml`, the configuration file of Integration, to subscribe to the blockchain data generated from the above smart contract.

## henesis.yaml

Let's create a `henesis.yaml` by using the Henesis CLI.

```bash
$ henesis init
```

If the above command runs successfully, the following `henesis.yaml`will be created.

```bash
sample/
├── migrations
├── contracts
├── tests
├── truffle-config.js
├── henesis.yaml
```

{% tabs %}
{% tab title="henesis.yaml" %}
```yaml
name: sample
version: v1    # (TBD) The version of this yaml file.
apiVersion: v1 # (TBD) The version of Henesis api. The type of message you receive can be changed depending on this version.

blockchain:
  platform: ethereum
  network: mainnet
  threshold: 12

filters:
  contracts:
    - address: '0x'
      name: example
      files: # The events of the contracts listed below can be combined together at this address.
        - path: ./contracts/example.sol
          contractName: example
          compilerVersion: 0.5.8

provider:
  type: webSocket
  timeout: 10000 # optional. (default: 10000, unit: ms)
# if you want to use webhook, you need to place it
#  type: webhook
#  url: https://localhost:8080
#  method: POST
#  headers:
#    Authorization: 'Bearer YOUR-OWN-TOKEN'
```
{% endtab %}
{% endtabs %}

Here, you can specify following information.

### Basic Information

* `name` :  a name of integration. Cannot be duplicated.
* `version` :  a version of integration.
* `apiVersion` :  a version of Henesis API. The type of message you receive can be changed depending on this version.

### Blockchain

Define which platform and network you would like to subscribe to. Platform we support can be found in the [FAQ](faq/supported-blockchains.md).

* `platform`: A platform of blockchain\(e.g. ethereum, klaytn\).
* `network`: A network of blockchain\(e.g. mainnet, ropsten\).
* `threshold`: Block confirmation times. In case of chain re-organization, blockchain data are delivered after the blockchain `threshold` has been reached.

### Filters

Define which type of blockchain data you want to subscribe to. Currently, the Henesis supports subscribing to events from the specified smart contracts. More types will be added in the future.

#### Contracts

Define an information of smart contracts you want to subscribe to. 

* `address`: An address of the deployed smart contract.
* `name`: A name of the contract filter.

#### Files

Define an information of smart contract files you want to subscribe to.

* `path`: A file path of the smart contract source code.
* `contractName`: A name of the contract to subscribe in the source code
* `compilerVersion`: A solidity compiler version. Must be identical to the compiler version at the time of deployment.

{% hint style="info" %}
To subscribe the events that occur at that contract address, you should register a filter for each contract address. If the event you want to subscribe is caused by delegatecall, you need to fill out both the caller and callee contract information in the **files** field. Refer to the [FAQ](https://docs.henesis.io/faq/delegatecall) for details.
{% endhint %}

### Provider

Define how to deliver blockchain data from the Henesis to you.

**webSocket**

* `type` : Way for delivering data. Henesis supports both `webSocket` and `webhook`

**webhook**

* `type` : Way for delivering data. Henesis supports both `webSocket` and `webhook`
* `url`: URL to deliver
* `method`: HTTP method\(e.g. POST, PUT, GET\)
* `header`: HTTP header. You can configure header key and value like`Authorization: 'Bearer YOUR-OWN-TOKEN'`. - _optional_

## Deployment

After you set up the `henesis.yaml`, you can deploy the integration using the Henesis CLI.

```bash
$ henesis integration:deploy
```

You can check if the integration has been successfully deployed by using the command below.

```bash
$ henesis integration:status
Id              Name      Platform     Network   Version   Provider     State      
sample-ahfkr    sample    ethereum     mainnet   v1        webSocket    Available  
```

| Column | Description |
| :--- | :--- |
| Id | Integration Id |
| Name | Integration Name |
| Platform | Blockchain Platform \(ex. ethereum, klaytn \) |
| Network | Blockchain Network \(ex. mainnet, ropsten, rinkeby\) |
| Version | Integration Version |
| Provider | A way of delivering data. \(ex. webSocket, webhook\) |
| State | Integration Status. \(ex. Available, Unavailable\) |



