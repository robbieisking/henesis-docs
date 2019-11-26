---
description: Henesis Integrationとは何か、どのように配置するかについて学びます。
---

# Deploy Integrationについて

##  Integration

Integrationは、Henesisがブロックチェーンでイベントを取得し、ユーザに情報を伝達する際にイベントが格納される場所です。Integrationを展開するためには、以下の情報を指定する必要があります。

* 取得したいスマートコントラクトに関連する情報（住所、コントラクトファイルなど）
* ブロックチェーンの関連情報（イーサネットリウムメインネット、テストネットなど）
* イベントの取得方法（Webhook、WebSocket）

## 前提条件

Integrationを設定するには、以下の前提を満たす必要があります。

* [Henesis CLIのインストール](https://docs.henesis.io/v/ko/installation/henesis-cli)が完了している
* スマートコントラクトがブロックチェーンに配置されている

{% hint style="info" %}
このセクション以降では、Henesis CLIがインストールされ、既に展開されているスマートコントラクトがあることを前提としています。CLIの[インストール](https://docs.henesis.io/v/ko/installation/henesis-cli)ついては[こちら](https://docs.henesis.io/v/ko/installation/henesis-cli)を参照してください。
{% endhint %}

Truffleを使用してスマートコントラクトを作成した場合、ディレクトリ構造は以下のようになっています。

```bash
sample/
├── migrations
├── contracts
    └── example.sol
├── tests
├── truffle-config.js
```

上記のスマートコントラクトにおいて発生するブロックチェーンのデータを取得するためにIntegrationの設定ファイルであるhenesis.yamlをどのように作成するか学びましょう。

## henesis.yaml

Henesis CLIを使用して、henesis.yamlを作成してみましょう。

{% code title="" %}
```bash
$ henesis init
```
{% endcode %}

上記のコマンドが正常に実行されている場合、ディレクトリの下記の位置にhenesis.yamlが作成されています。

```bash
sample/
├── migrations
├── contracts
├── tests
├── truffle-config.js
├── henesis.yaml
```

{% code title="henesis.yaml" %}
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
# if you want to use webhook, you need to place it
#  type: webhook
#  url: https://localhost:8080
#  method: POST
#  headers:
#    Authorization: 'Bearer YOUR-OWN-TOKEN'
```
{% endcode %}

また、設定ファイルでは以下のような情報を設定することができます。

### Basic Information

* `version` :  Integrationのバージョン
* `name` :  Integrationの名前（他のintegrationと重複不可）

### Blockchain

どのプラットフォームおよびネットワークからデータを取得するか設定します。 Henesisがサポートしているプラットフォームのリストは[FAQ](https://docs.henesis.io/v/ko/faq/supported-blockchains)で確認できます。

* `platform`: ブロックチェーンのプラットフォーム名（例：ethereum, klaytn）
* `network`: ブロックチェーンネットワーク、メインネットやテストネット（例：mainnet, ropsten）
* `threshold`: ブロックの確認回数の閾値。チェーンの再編成の際、`threshold`に達した後にブロックチェーンのデータが転送されます

### Filters

取得するブロックチェーンのデータタイプを定義できます。現在は、サポート対象に指定されているスマートコントラクトのみのイベント取得をサポートしています。今後、他のタイプ（EOA等）も追加される予定です。

#### Contracts

取得したいスマートコントラクトの情報を入力します。

* `address`: 配置されているコントラクトのアドレス
* `name`: A name of the contract filter.

#### Files

Define an information of smart contract files you want to subscribe to.

* `path`: コントラクトのソースコードが記載されたファイルまでのパス
* `contractName`: A name of the contract to subscribe in the source code
* `compilerVersion`: ソリディティールコンパイラのバージョン。配布された当時のコンパイラのバージョンと一致させる必要があります。

{% hint style="info" %}
To subscribe the events that occur at that contract address, you should register a filter for each contract address. If the event you want to subscribe is caused by delegatecall, you need to fill out both the caller and callee contract information in the **files** field. Refer to the [FAQ](https://docs.henesis.io/faq/delegatecall) for details.
{% endhint %}

### Provider

Henesisが収集したデータをどのように配布するかを入力します。

* `type` : 配布するデータの形式、`webSocket`、`webhook`をサポートしています。
* `timeout`: メッセージ確認の待機時間（単位：ミリ秒）。 `timeout`で設定された時間内にHenesis SDKからACKが送信されない場合は、メッセージが正常に配信されなかったと判断して再送信します。デフォルトは10000msです。- webSocket、任意
* `url`: Henesisが呼び出すURLです。 - webhook
* `method`: HTTP メソッド（POST、PUT、GETなど） - webhook
* `header`: HTTP ヘッダー。 `Authorization：'Bearer YOUR-OWN-TOKEN'`のようにしてKeyとValueを設定することができます。 - webhook、任意

## Deployment

`henesis.yaml`を設定した後、Henesis CLIを使用して、Integrationを配布することができます。

{% code title="" %}
```bash
$ henesis integration:deploy
```
{% endcode %}

Integrationが正常に配布されているかは、次のコマンドを使用して確認することができます。

{% code title="" %}
```bash
$ henesis integration:status
Id              Name      Platform     Network   Version   Provider     State      
sample-ahfkr    sample    ethereum     mainnet   v1        webSocket    Available  
```
{% endcode %}

| Column | Description |
| :--- | :--- |
| Id | IntegrationのID |
| Name | Integrationの名前 |
| Platform | ブロックチェーンプラットフォーム（例：ethereum, klaytn） |
| Network | ブロックチェーンネットワーク \(例： mainnet, ropsten, rinkeby\) |
| Version | Integrationバージョン |
| Provider | データ転送方式（例：webSocket, webhook） |
| State | Integrationの状態（例：Available, Unavailable） |



