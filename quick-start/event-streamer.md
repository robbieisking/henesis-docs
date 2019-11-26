# Event Streamer

## Step 1：Henesis CLIをインストールする

### インストール

```bash
npm install -g @haechi-labs/henesis-cli
```

### Henesisアカウントでログイン

```bash
henesis login
```

## Step 2：サンプルコードを取得

Sample Repositoryを使用してHenesisがどのようにブロックチェーンのデータを失わずに購読することができるかどうかについて説明します。

### Sample Repositoryインポート

```
git clone https://github.com/HAECHI-LABS/sample-erc20-watcher
```

### Sampleディレクトリに移動

```
cd sample-erc20-watcher
```

### Dependencyインストール

```
npm install
```

## Step 3：ブロックチェーンデータのフィルタを登録する

Henesisを利用してブロックチェーンのデータを失わずに購読するためには、いくつかのコントラクトのイベントを聞くか、フィルタに記録する過程が必要です。フィルタ登録は`henesis.yaml`を介して行うことができ、サンプルコードでは、イーサネットリウムメインネットに配布されている[Tether Token contract](https://etherscan.io/address/0xdac17f958d2ee523a2206206994597c13d831ec7)のイベントをサブスクライブすることを介してHenesis使い方を説明します。

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

## Step 4：Integration配布する

CLIのコマンドを使用して[integration](https://docs.henesis.io/v/ko/subscribing-events/deploy-integration#integration)を配布することができます。

```bash
henesis integration:deploy
```

正常にintegrationが配布された場合は、次のような内容が表示されます。

```text
Deploying... !
tether-tutorial-jrweu has been deployed
Deploying... done
```

CLIのコマンドを使用してintegrationの状態を確認することができ、`State`が`Available`と表示されている場合、通常integrationが展開されています。

```bash
henesis integration:status
```

```text
Id                     Name             Platform  Network  Version  Provider   State
tether-tutorial-jrweu  tether-tutorial  ethereum  mainnet  v1       webSocket  Available
```

## Step 5：Eventデータを確認する

### ClientIdとIntegrationId確認と設定する

HenesisでEventデータをサブスクライブするには、次のような情報が必要です。

* `CLIENT_ID`：`henesis account：describe`で確認することができます。 
* `INTEGRATION_ID`：`henesis integration：status`で確認することができます。

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

 Enter the ClientId and IntegrationId in the `.env` configuration file.

設定ファイルである`.env`にCLIコマンドを使用して確認したClientIdとIntegrationIdを記入します。

{% tabs %}
{% tab title=".env" %}
```javascript
CLIENT_ID=<your client id>
INTEGRATION_ID=<your integration id>
```
{% endtab %}
{% endtabs %}

### サンプルコードをビルドして実行する

npmを介してソースコードをビルドします。

```bash
$ npm run build:standalone
```

APIサーバーを実行します。

```bash
$ node index
```

ブラウザで[http://locahost:3000](http://locahost:3000)を接続してEventデータをよく購読していることを確認します。

![](../.gitbook/assets/2019-10-16-11.09.49.png)

{% hint style="info" %}
購読するブロックチェーンデータのraw dataは、[そのリンク](https://docs.henesis.io/v/ko/faq/json-schema)を参照してください。
{% endhint %}

