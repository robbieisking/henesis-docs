# Trusted Node

## Step 1: Install Henesis CLI

### Installation

```bash
npm install -g @haechi-labs/henesis-cli
```

### Login with your Henesis account

```bash
henesis login
```

## Step 2: Check your clientId through CLI

You can get **client ID** through [Henesis CLI.](https://docs.henesis.io/installation/henesis-cli)

```text
henesis account:describe
```

Copy your `<YOUR-CLIENT-ID>` for the authentication.

```bash
Email: hello@haechi.io
Name: hello
Organization: haechi
clientId: <YOUR-CLIENT-ID>
```

## Step 3: Try Trusted Node API

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

If you call `eth_blockNumber` method through above api request, you can get the following response.

```bash
{
    "jsonrpc": "2.0",
    "result": "0x8960fc",
    "id": 1
}
```

