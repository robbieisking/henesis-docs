# DelegateCallのイベントデータもサブスクライブすることができますか？

Henesisを通じてdelegateCallで発生したイベントも購読することができます。購読方法は、コントラクトフィルタの**files**フィールドにcallerコントラクトとcalleeコントラクトについての情報をすべて記録してくれればされます。A ContractでB Contractの`plus`という名前の関数をdelegateCallする場合を例にどのようにイベントをサブスクライブすることができるか説明します。

![](../.gitbook/assets/delegatecall.png)

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

* AContractの`delegateEventCall`関数はcalleeコントラクトであるBContractのアドレスを引数として受け`plus`関数をdelegateCallます。

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

* BContractのplus関数は`SampleEvent`イベントをemitします。

If you want to subscribe delegate Call event from A Contract to Contract、fill out the address of the caller contract、AContract、in the address field of the contract filter in the `henesis.yaml` file。

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

