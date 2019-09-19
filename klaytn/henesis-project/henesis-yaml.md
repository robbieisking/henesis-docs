# henesis.yaml 설정

이제 어떤 스마트 컨트랙트를 구독할지 Henesis에 알려야합니다. 이를 위해 프로젝트에서 `henesis.yaml` 파일을 작성해야합니다. 해당 파일에는 메타정보를 포함하여 구독 대상, 구독 방식 등이 작성됩니다.‌

## Version, Name <a id="version-name"></a>

버전과 이름은 해당 프로젝트를 식별하는 구분자로 사용됩니다. 이름은 영어 소문자, 숫자, '-' 그리고 '.'로만 구성되어야 하며, 최대 길이는 253자입니다.‌

본 예제에서는 버전은 `v1` 로, 프로젝트 이름은 `kitty-tutorial` 로 지정하였습니다.

{% code-tabs %}
{% code-tabs-item title="henesis.yaml" %}
```yaml
version: v1
name: kitty-tutorial
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* `version`: 설정 파일의 버전.
* `name`: 해당 프로젝트의 이름.

## Contract Filters <a id="contract-filters"></a>

설정 파일의 해당 영역에서는 Henesis를 통해 구독하고자 하는 스마트 컨트랙트들의 정보를 기입합니다. 아래와 같이 `filters` 의 `contract` 속성 내에 크립토키티 스마트 컨트랙트들 정보를 적습니다.

{% code-tabs %}
{% code-tabs-item title="henesis.yaml" %}
```yaml
...
filters:
  contract:
  - address: '0x20A1CFF753ae36988f73507A5497f02369970D70'
    path: ./contracts/KittyCore.sol
    name: KittyCore
    compilerVersion: 0.4.18
  - address: '0xD1dFd8F3bd2f05D497296Fc1C44Ff8b397189a03'
    path: ./contracts/SaleClockAuction.sol
    name: SaleClockAuction 
    compilerVersion: 0.4.18
  - address: '0xC1C88A7B90062866F54320b18Ee5DA65F79202D6'
    path: ./contracts/SiringClockAuction.sol
    name: SiringClockAuction 
    compilerVersion: 0.4.18
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* `address`: 블록체인에 배포된 스마트 컨트랙트의 주소
* `path`: 솔리디티 파일의 저장 경로
* `name`: 구독할 스마트 컨트랙트 명
* `compilerVersion`: 배포된 스마트 컨트랙트의 원본파일이 컴파일 될때 사용된 컴파일러의 버전

**`name`**속성은 **`path`**가 가리키는 솔리디티 파일 내에 존재하는 스마트 컨트랙트 명이어야 합니다.‌

## **Blockchain** <a id="blockchain"></a>

**Blockchain** 파트에서는 구독하고자 하는 스마트 컨트랙트가 배포된 블록체인의 플랫폼과 네트워크 명을 기술하는 영역입니다. 튜토리얼에서는 **클레이튼 바오밥 테스트넷**에 배포된 크립토키티 스마트 컨트랙트를 구독하므로, 다음과 같이 설정합니다.

{% code-tabs %}
{% code-tabs-item title="henesis.yaml" %}
```yaml
...
blockchain:
  platform: klaytn 
  network: baobab
  threshold: 1
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* `platform`: 블록체인 플랫폼의 종류 \(ex. ethereum, klaytn\)
* `network`: 블록체인 플랫폼의 네트워크 종류 \(ex. mainnet, ropsten, rinkeby, baobab\)
* `threshold` : 최소 블록 컨펌 수

{% hint style="warning" %}
주의: 블록체인 데이터를 가져올때`threshold`만큼의 블록 컨펌을 기다립니다.  따라서 실제 데이터와 Henesis로 들어오는 데이터는 시간 차이가 존재합니다.
{% endhint %}

{% hint style="info" %}
현재 Henesis는 아래의 다섯 가지 체인을 지원합니다. 추후, 다른 체인들\(ex. kovan, Görli etc\)을 지원할 예정입니다.
{% endhint %}

| `platform` | `network` | 체인 |
| :---: | :---: | :---: |
| ethereum | mainnet | 이더리움 메인넷 |
| ethereum | ropsten | 이더리움 ropsten 테스트넷 |
| ethereum | rinkeby | 이더리움 rinkeby 테스트넷 |
| klaytn | mainnet | 클레이튼 cypress 메인넷 |
| klaytn | baobab | 클레이튼 baobab 테스트넷 |

## Provider <a id="provider"></a>

Henesis로 부터의 이벤트를 전달 받을 방법을 선택하는 곳입니다. Henesis는 Webhook과 WebSocket을 지원하고 있습니다. 이번 튜토리얼에서는 WebSocket을 이용할 것이므로 다음과 같은 설정을 추가해줍니다.

{% code-tabs %}
{% code-tabs-item title="henesis.yaml" %}
```yaml
...
provider:
  type: webSocket
```
{% endcode-tabs-item %}
{% endcode-tabs %}



