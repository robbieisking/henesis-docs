---
description: Henesis Integration이란 무엇이고 어떻게 배포하는지 알아봅시다.
---

# Deploy Integration

## Integration이란?

Integration은 Henesis가 블록체인에서 이벤트를 듣고, 유저에게 정보를 전달하는 컨테이너 단위입니다. Integration을 배포하기 위해서 유저는 아래 정보를 지정해야 합니다.

* 구독하고 싶은 스마트 컨트랙트에 관련된 정보\(주소, 컨트랙트 파일 등\)
* 블록체인 관련 정보\(이더리움 메인넷, 테스트넷 등\)
* 이벤트를 전달 받을 방법\(Webhook, WebSocket\)

## Prerequisite

Integration을 설정하기 위해서는 아래와 같은 사전 조건이 필요합니다.

* [Henesis CLI 설치](../installation/henesis-cli.md)
* 블록체인에 배포된 스마트 컨트랙트

{% hint style="info" %}
지금부터 Henesis CLI가 설치됐고, 이미 배포된 스마트 컨트랙트가 있다는 가정 하에 설명하겠습니다.   
CLI 설치에 관한 안내 사항은 [링크](../installation/henesis-cli.md)를 참고하세요.  
{% endhint %}

[Truffle](https://www.trufflesuite.com/truffle)을 사용해서 스마트 컨트랙트를 작성했다면 아래와 같은 구조의 디렉토리가 있습니다. 

```bash
sample/
├── migrations
├── contracts
    └── example.sol #배포된 컨트랙트의 소스코드 
├── tests
├── truffle-config.js
```

위의 스마트 컨트랙트들에서 발생하는 블록체인 데이터를 듣기 위해 Integration의 설정 파일인 `henesis.yaml`을 어떻게 작성하는지 알아봅시다.

## henesis.yaml

Henesis CLI를 이용하여 henesis.yaml을 생성하여 봅시다.

{% tabs %}
{% tab title="" %}
```bash
$ henesis init
```
{% endtab %}
{% endtabs %}

위 명령어가 성공적으로 실행 됐다면, 아래와 같은 `henesis.yaml` 이 생성 되었을 것입니다.

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
version: v1
name: sample

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

아래와 같은 정보를 설정할 수 있습니다.

### Basic Information

Integration의 기본 정보들을 입력합니다. 

* `version` :  Integration의 버전 
* `name` : Integration의 이름 \(이름은 중복될 수 없습니다.\) 

### Blockchain

어떤 블록체인 네트워크로부터 데이터를 가져올지 입력합니다. Henesis가 지원하는 블록체인 목록은 [FAQ](../faq/supported-blockchains.md)에서 확인할 수 있습니다.

* `platform`: 블록체인 플랫폼. 
* `network`: 블록체인 네트워크, 메인넷 또는 테스트넷을 설정할 수 있습니다. 
* `threshold`: Chain Reorganization을 고려하여, 해당 블록에서 `threshold`만큼의 블록이 추가적으로 생성된 뒤 블록체인 데이터를 전달합니다.

### Filters

어떤 블록체인 데이터를 읽어들일지 입력합니다. 현재는 지정된 스마트 컨트랙트에서 발생하는 이벤트를 필터링합니다. 추후에 EOA와 같은 다른 타입들이 추가될 예정입니다.

#### Contracts

구독하려는 스마트 컨트랙트 정보를 입력합니다.

* `address`: 배포된 컨트랙트의 주소
* `name`: 컨트랙트 필터의 이름

#### Files

구독하려는 스마트 컨트랙트 파일에 대한 정보를 입력합니다.

* `path`: 배포된 컨트랙트의 소스코드가 있는 파일 경로
* `contractName`: 해당 소스코드에서 구독하고자 하는 컨트랙트의 이름
* `compilerVersion`: 솔리디티 컴파일러 버전. 배포된 당시에 컴파일러 버전과 일치해야 합니다.

{% hint style="info" %}
해당 컨트랙트 주소에서 발생하는 이벤트를 구독하려면 각 컨트랙트 주소마다 필터를 등록해주어야 합니다. 만약 구독하려는 이벤트가 delegateCall로 인해 발생한다면 **files**에 caller 컨트랙트와 callee 컨트랙트의 정보를 모두 기입해주어야 합니다. 자세한 방법은 [FAQ](https://docs.henesis.io/v/ko/faq/delegatecall)를 참고해주세요.
{% endhint %}

### Provider

Henesis가 수집한 데이터를 어떤 방식으로 전달할지 입력합니다.

**webSocket**

* `type` : 데이터를 전달할 방식, `webSocket`, `webhook`을 지원합니다.

**webhook**

* `type` : 데이터를 전달할 방식, `webSocket`, `webhook`을 지원합니다.
* `url`: Henesis가 호출할 URL.
* `method`: HTTP method\(POST, PUT, GET 등\)
* `headers`: HTTP header. `Authorization: 'Bearer YOUR-OWN-TOKEN'`와 같이 원하는 Key와  Value를 설정할 수 있습니다. - _optional_

## Deployment

`henesis.yaml` 을 설정한 후 Henesis CLI를 이용하여 Integration을 배포할 수 있습니다.

{% tabs %}
{% tab title="" %}
```bash
$ henesis integration:deploy
```
{% endtab %}
{% endtabs %}

Integration이 성공적으로 배포 됐는지 아래의 명령어를 통해 확인할 수 있습니다.

```bash
$ henesis integration:status
Id              Name      Platform     Network   Version   Provider     State      
sample-ahfkr    sample    ethereum     mainnet   v1        webSocket    Available  
```

| Column | Description |
| :--- | :--- |
| Id | Integration Id |
| Name | Integration 이름 |
| Platform | 블록체인 플랫폼 \(ex. ethereum, klaytn\) |
| Network | 블록체인 네트워크 \(ex. mainnet, ropsten, rinkeby 등\) |
| Version | Integration 버전 |
| Provider | 데이터 전달 방식 \(ex. webSocket, webhook\) |
| State | Integration 상태 \(ex. Available, Unavailable\) |



