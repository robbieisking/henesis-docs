# 배포하기

이제 모든 설정이 완료되었습니다. 현재까지 작성된 `henesis.yaml` 파일은 다음과 같습니다.

{% code-tabs %}
{% code-tabs-item title="henesis.yaml" %}
```yaml
version: v1
name: kitty-tutorial

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

blockchain:
  platform: klaytn
  network: baobab
  threshold: 1
  
provider:
  type: webSocket
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 배포 설정 확인하기

Henesis에서는 프로젝트의 단위를 "**integration**"이라고 지칭합니다. CLI의 integration 명령어를 이용해 현재 로그인된 계정으로 배포 integration의 상태들을 조할 수 있습니다. 첫번째 프로젝트를 배포하기 전 상태를 확인해보면 아무 것도 배포되지 않은 것을 확인할 수 있습니다.

```bash
$ henesis integration:status
Id     Name     Platform     Network   Version   Provider     State
```

## Integration 배포하기

이제 아래 명령어를 입력하여 integration을 배포할 수 있습니다.

```text
$ henesis integration:deploy
kitty-tutorial-hptnh has been deployed
```

배포된 integration의 이름은 \[Project Name\]-\[Arbitrary String\]의 구조로 생성됩니.

{% hint style="info" %}
배포 명령어는 henesis.yaml 파일이 존재하는 경로에서만 실행하여야 합니다.
{% endhint %}

## 배포한 Integration 확인하기

Integration을 배포한 후에 `integration:status` 명령어를 이용하여 확인해보면 다음과 같이 배포된 integration의 정보와 상태를 확인할 수 있습니다.

```text
$ henesis integration:status
Id                      Name              Platform     Network   Version   Provider     State
kitty-tutorial-hptnh    kitty-tutorial    klaytn       baobab    v1        webSocket    Available
```

상태 정보\(state\)가 `Available` 이 되면, 이 integration은 사용할 준비가 되었습니다!

