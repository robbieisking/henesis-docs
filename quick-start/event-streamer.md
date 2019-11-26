# Event Streamerについて

## Step 1：Henesis CLIをインストールする

### インストール

```bash
npm install -g @haechi-labs/henesis-cli
```

### Henesisアカウントでログイン

```bash
henesis login
```

## Step 2：サンプルコードを取得する

Sample Repositoryを使用してHenesisがどのようにブロックチェーンのデータを失わずに購読することができるかどうかについて説明します。

### Sample Repositoryのインポート

```
git clone https://github.com/HAECHI-LABS/sample-erc20-watcher
```

### Sampleディレクトリに移動

```
cd sample-erc20-watcher
```

### 依存関係にあるツールのインストール

```
npm install
```

## Step 3：ブロックチェーンデータフィルタを登録する

Henesisを利用してブロックチェーンのデータをもれなく取得するためには、どのコントラクトのイベントがリッスンされそうかをフィルタに記録する過程が必要です。フィルタの登録は`henesis.yaml`を介して行うことができ、サンプルコードでは、イーサネットリウムメインネットに配布されている[Tether Token contract](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7)のイベントを取得する流れを使ってHenesisの使い方を説明します。

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

## Step 4：Integrationをデプロイする

CLIのコマンドで[integration](https://docs.henesis.io/v/ko/subscribing-events/deploy-integration#integration)をデプロイすることができます。

```bash
henesis integration:deploy
```

正常にintegrationがデプロイできた場合、以下のように表示されます。

```text
Deploying... !
tether-tutorial-jrweu has been deployed
Deploying... done
```

CLIのコマンドでintegrationの状態を確認することができ、`State`が`Available`と表示されている場合、通常はintegrationが展開されています。

```bash
henesis integration:status
```

```text
Id                     Name             Platform  Network  Version  Provider   State
tether-tutorial-jrweu  tether-tutorial  ethereum  mainnet  v1       webSocket  Available
```

## Step 5：Eventデータを確認する

### ClientIdとIntegrationId確認と設定する

HenesisでEventデータを取得するには、次のような情報が必要です。

* `CLIENT_ID`：`henesis account：describe`コマンドで確認することができます。 
* `INTEGRATION_ID`：`henesis integration：status`コマンドで確認することができます。

ClientIdは以下のようなCLIコマンドを使用して確認することができます。

```text
henesis account:describe
```

```text
Email: haechi@haechi.io
Name: haechi
Organization: haechi-labs
ClientId: a481485a958f1b82ac210ec4eea27943
```

IntegrationIdは以下のようなCLIコマンドを使用して確認することができます。

```bash
henesis integration:status
```

```text
Id                     Name             Platform  Network  Version  Provider   State
tether-tutorial-jrweu  tether-tutorial  ethereum  mainnet  v1       webSocket  Available
```

設定ファイルである`.env`にCLIコマンドを使用して、先ほど確認したClientIdとIntegrationIdを記入します。

{% tabs %}
{% tab title=".env" %}
```javascript
CLIENT_ID=<your client id>
INTEGRATION_ID=<your integration id>
```
{% endtab %}
{% endtabs %}

### サンプルコードをビルドして実行する

npmコマンドでソースコードをビルドします。

```bash
$ npm run build:standalone
```

APIサーバーを実行します。

```bash
$ node index
```

ブラウザで[http://locahost:3000](http://locahost:3000)にアクセスし、Eventデータを正しく取得できていることを確認します。

![](../.gitbook/assets/2019-10-16-11.09.49.png)

{% hint style="info" %}
取得するブロックチェーンデータの生データについては[こちらのリンク](https://docs.henesis.io/v/ko/faq/json-schema)を参照してください。
{% endhint %}

