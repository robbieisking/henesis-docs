---
description: Subscribe blockchain data without loss in real-time
---

# Introduction

Have you ever faced any problems which are listed below while developing DApps\(Decentralized Applications\)?‌

* I'm not sure that I have subscribed all data from blockchain without loss. 
* It's difficult to check whether transactions are mined or not.
* It's hard to sync with blockchain because of frequent disconnections from the node.

Henesis provides following features to address above mentioned problems.‌

* Listen, filter, parse and deliver blockchain data without loss in real-time
* Reorganization Tolerance.
* Fully-managed stable blockchain node.

Without Henesis, you have to‌

* Manage a stable blockchain node by yourself.
* Write your own code for subscribing to blockchain events.
* Manually deal with a chain reorganization.
* Inspect a transaction pool or a latest block for tracking transactions.

Sounds terrible, doesn't it?‌

From now on, Henesis will take care of everything instead of you.

## What is a blockchain event?‌

Subscription to events data is the easiest way to monitor the state of smart contracts.

```javascript
contract Token is ERC20 {
    ...
    function transfer(address recipient, uint256 amount) external returns (bool){
     ...
     emit Transfer(msg.sender, recipient, amount);
    };

    event Transfer(address indexed from, address indexed to, uint256 value);
}
```

You can trace all transfers that have occurred in the smart contract by subscribing to `Transfer` event.‌

## Benefits of using the Henesis <a id="benefits-of-using-the-henesis"></a>

‌In order to subscribe to blockchain events and provide events data to users, it is common to operate an Event Subscriber. Many DApps subscribe to events from the blockchain through the following architectures.‌

![](../.gitbook/assets/2019-10-10-1.29.08.png)

If a blockchain application integrate with Henesis, there is no longer need to operate the **Blockchain Node** and **Event Subscriber** manually. It means you won't need to write web3 codes for receiving blockchain data from blockchain node that third-party providers\(ex. Infura, Alchemy\) offer.



![](../.gitbook/assets/2019-10-10-1.33.31.png)

