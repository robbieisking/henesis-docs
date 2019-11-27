# Trusted Node

## Step 1: Henesis CLI 설치하기

### Henesis CLI 설치‌

```bash
npm install -g @haechi-labs/henesis-cli
```

### Henesis 계정으로 로그인

```bash
henesis login
```

## Step 2: CLI를 통해 clientId 확인하기

[Henesis CLI](https://docs.henesis.io/installation/henesis-cli) 의 다음과 같은 명령어를 통해 **client ID** 를 확인할 수 있습니다.

```bash
henesis account:describe
```

인증을 위해 `<YOUR-CLIENT-ID>` 를 복사하세요.

```bash
Email: hello@haechi.io
Name: hello
Organization: haechi
clientId: <YOUR-CLIENT-ID>
```

## Step 3: Trusted Node API 사용하기

* web3.js

```javascript
const Web3 = require('web3');
const web3 = new Web3("https://tn.henesis.io/ethereum/mainnet?clientId=<YOUR-CLIENT_ID>");
```

* curl

```bash
curl -X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc": "2.0", "id": 1, "method": "eth_blockNumber", "params": []}' \
"https://tn.henesis.io/ethereum/mainnet?clientId=<YOUR-CLIENT_ID>"
```

위의 api 요청을 통해 `eth_blockNumber` 메소드를 호출하면, 아래와 같은 응답을 받을 수 있습니다.

```bash
{
    "jsonrpc": "2.0",
    "result": "0x8960fc",
    "id": 1
}
```

