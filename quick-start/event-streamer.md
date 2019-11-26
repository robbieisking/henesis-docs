# Event Streamer

## Step 1: Henesis CLI 설치하기 <a id="step-1-install-henesis-cli"></a>

### Henesis CLI 설치‌

```bash
npm install -g @haechi-labs/henesis-cli
```

### Henesis 계정으로 로그인

```bash
henesis login
```

## Step 2: 샘플 코드 가져오기 <a id="step-2-clone-the-sample-code"></a>

Sample Repository를 통해 어떻게 Henesis로 블록체인 데이터를 유실없이 구독할 수 있는지 설명합니다.

### Sample Repository 가져오기

```
git clone https://github.com/HAECHI-LABS/sample-erc20-watcher
```

### Sample 디렉토리로 이동

```
cd sample-erc20-watcher
```

### dependency 설치

```
npm install
```

## Step 3: 블록체인 데이터 필터 등록하기 <a id="step-3-register-blockchain-data-filter"></a>

Henesis를 이용하여 블록체인의 데이터를 유실없이 구독하기 위해서는 어떤 컨트랙트의 이벤트를 들을 것인지 필터에 기록하는 과정이 필요합니다. 필터 등록은 `henesis.yaml` 을 통해 할 수 있으며, 샘플 코드에서는 이더리움 메인넷에 배포되어 있는 [TetherToken 컨트랙트](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7)의 이벤트를 구독하는 것을 통해 Henesis 사용법을 설명합니다.

{% tabs %}
{% tab title="henesis.yaml" %}
```yaml
version: v1
name: tether-tutorial

blockchain:
  platform: ethereum
  network: mainnet
  threshold: 6  # optional.
                # ethereum: (default: 12, min: 6)
                # klaytn: (default: 0, min: 0)

filters:
  contracts:
    - address: '0xdac17f958d2ee523a2206206994597c13d831ec7'
      name: TetherToken
      files: # The events of the contracts listed below can be combined together at this address.
        - path: ./contracts/TetherToken.sol
          contractName: TetherToken
          compilerVersion: 0.4.18

provider:
  type: webSocket
```
{% endtab %}
{% endtabs %}

## Step 4: Integration 배포하기 <a id="step-4-deploy-your-integration"></a>

CLI의 명령어를 통해 [integration](https://docs.henesis.io/v/ko/subscribing-events/deploy-integration#integration)을 배포할 수 있습니다.

```bash
henesis integration:deploy
```

성공적으로 integration이 배포되었다면 다음과 같은 내용이 표시됩니다.

```text
Deploying... !
tether-tutorial-jrweu has been deployed
Deploying... done
```

CLI의 명령어를 통해 integration의 상태를 확인할 수 있으며, `State`가 `Available`로 표시되었으면 정상적으로 integration이 배포된 상태입니다.

```bash
henesis integration:status
```

```text
Id                     Name             Platform  Network  Version  Provider   State
tether-tutorial-jrweu  tether-tutorial  ethereum  mainnet  v1       webSocket  Available
```

## Step 5: Event 데이터 확인하기 <a id="step-5-check-for-the-event-data"></a>

### ClientId 와 IntegrationId 확인 및 설정하기

Henesis를 통해 Event 데이터를 구독하기 위해서는 다음과 같은 정보들이 필요합니다.

* `CLIENT_ID` : `henesis account:describe`를 통해 확인할 수 있습니다. 
* `INTEGRATION_ID` : `henesis integration:status`를 통해 확인할 수 있습니다. 

ClientId는 아래와 같은 CLI 명령어를 통해 확인할 수 있습니다. 

```text
henesis account:describe
```

```text
Email: haechi@haechi.io
Name: haechi
Organization: haechi-labs
ClientId: a481485a958f1b82ac210ec4eea27943
```

IntegrationId는 아래와 같은 CLI 명령어를 통해 확인할 수 있습니다.

```bash
henesis integration:status
```

```text
Id                     Name             Platform  Network  Version  Provider   State
tether-tutorial-jrweu  tether-tutorial  ethereum  mainnet  v1       webSocket  Available
```

 설정파일인 `.env` 에 CLI 명령어를 통해 확인한 ClientId와 IntegrationId를 기입해줍니다.

{% tabs %}
{% tab title=".env" %}
```javascript
CLIENT_ID=<your client id>
INTEGRATION_ID=<your integration id>
```
{% endtab %}
{% endtabs %}

### 샘플 코드 빌드 및 실행하기

​npm을 통해 소스 코드를 빌드해줍니다.

```bash
$ npm run build:standalone
```

API 서버를 실행합니다.

```bash
$ node index
```

브라우저에서 [http://locahost:3000](http://locahost:3000) 를 접속해 Event 데이터를 잘 구독하고 있음을 확인합니다.

![](../.gitbook/assets/2019-10-16-11.09.49.png)

{% hint style="info" %}
구독하는 블록체인 데이터의 raw data는 [여기](https://docs.henesis.io/v/ko/faq/json-schema)를 참고하세요.
{% endhint %}

