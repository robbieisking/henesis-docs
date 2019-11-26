---
description: How to subscribe to blockchain events through Henesis?‌
---

# Introduction

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

