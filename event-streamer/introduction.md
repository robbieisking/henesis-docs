---
description: Henesis를 이용하여 블록체인 이벤트를 어떻게 구독할 수 있는지 알아봅시다.
---

# Introduction

## 블록체인 이벤트란?

블록체인에서 스마트 컨트랙트의 상태를 지속적으로 모니터링 하기 가장 쉬운 방법은 바로 이벤트를 듣는 것입니다. 

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

위와 같은 토큰 스마트 컨트랙트에서 `Transfer`이벤트를 구독한다면 토큰의 모든 전송을 추적할 수 있습니다.

## Henesis를 통한 이벤트 구독의 장점

블록체인 이벤트를 빠짐없이 구독하여 사용자에게 제공하기 위해서는 이벤트를 구독하는 서버\(Event Subscriber\)를 운영하는 것이 일반적입니다. 많은 블록체인 어플리케이션들이 아래와 같은 아키텍처를 통해 블록체인으로부터 이벤트를 구독합니다.

![](../.gitbook/assets/2019-10-10-1.29.08.png)

Henesis는 위의 Diagram에서 **Blockchain Node** 와 **Event Subscriber**를 대체할 수 있는 SaaS 입니다. Henesis를 사용하면 Web3 코드를 작성해 노드 제공 서비스들\(Infura, Alchemy 등\)의 Blockchain Node로부터 데이터를 가져오는 로직을 작성할 필요가 없습니다. 

![](../.gitbook/assets/2019-10-16-9.05.11%20%281%29.png)

자, 이제 Henesis를 통해 쉽게 이벤트를 구독하는 법을 알아봅시다.

