# Can the Henesis also subscribe to event data for delegatecall?

Henesis can also subscribe to events emitted by delegateCall. The way to subscribe is to fill out both the caller contract and the callee contract in the **files** field of the contract filter. For example, if Contract A executes delegateCall to the `plus` function in Contract B, it will explain how to subscribe to the event.

![](../../.gitbook/assets/delegatecall.png)

{% tabs %}
{% tab title="AContract.sol" %}
```javascript
pragma solidity ^0.4.23;

contract AContract {
    function delegateEventCall(address other1) public {
        require(other1.delegatecall(bytes4(keccak256("plus()"))));
    }
}
```
{% endtab %}
{% endtabs %}

* AContract's `delegateEventCall` function takes the address of the callee contract, BContract, as a function parameter and executes delegatecall to the `plus` function.

{% tabs %}
{% tab title="BContract.sol" %}
```javascript
pragma solidity ^0.4.23;

contract BContract {
    event SampleEvent();

    function plus() public {
        emit SampleEvent();
    }
}
```
{% endtab %}
{% endtabs %}

* BContract's plus function emits a `SampleEvent` event.

If you want to subscribe delegateCall event from AContract to BContract, fill out the address of the caller contract, AContract, in the address field of the contract filter in the `henesis.yaml` file.

{% tabs %}
{% tab title="henesis.yaml" %}
```yaml
version: v1
name: delegatecall-sample

blockchain:
  platform: ethereum
  network: mainnet
  threshold: 12 # optional.
                # Ethereum: (default: 12, min: 6)
                # Klaytn: (default: 0, min: 0)

filters:
  contracts:
    - address: '0x762a76234AB93Ff0E8AB4F462A53D91fA7E8c728'
      name: AContract
      files: # optional. The events of the contracts listed below can be combined together at this address.
        - path: ./contracts/AContract.sol
          contractName: AContract
          compilerVersion: 0.4.23
        - path: ./contracts/BContract.sol
          contractName: BContract
          compilerVersion: 0.4.23

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

