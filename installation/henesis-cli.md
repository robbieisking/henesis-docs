# Henesis CLI

## Henesis CLIのインストール

Henesis CLIツールを使用すれば、新規Henesisプロジェクトを迅速に作成し、プロジェクト開発を進めるためのコマンドを実行することができます。npmコマンドでHenesis CLIをインストールします。

```text
$ npm install -g @haechi-labs/henesis-cli
```

上記コマンドの実行によってCLIが正常にインストールされている場合は、次のコマンドを使用してインストール状態を確認することができます。

```bash
$ henesis
🚀 Command Line Interface tool to Utilize henesis

VERSION
  @haechi-labs/henesis-cli/1.0.0-beta.30 darwin-x64 node-v10.13.0

USAGE
  $ henesis [COMMAND]

COMMANDS
  account       manage your account
  autocomplete  display autocomplete installation instructions
  help          display help for henesis
  init          set up configuration file required for your project
  integration   manage integrations
  login         perform a login
  logout        perform a logout
  node          get node status
```

## Henesisアカウントでログインする <a id="henesis"></a>

{% hint style="info" %}
Henesisアカウント発行の申請は既にお済みですか？もしまだであれば、[hello@henesis.io](mailto:hello@henesis.io)までお気軽にご連絡ください。
{% endhint %}

新規プロジェクトを作成する前に、まず、CLIを使用してログインする必要があります。ログインは発行されたメールアドレスとパスワードを利用して行うことができます。

```bash
$ henesis login
```

上記のコマンドを実行すると、以下のような情報収集に対する同意画面が表示されます。

```bash
Allow Henesis to collect CLI usage and error reporting information
(y)es or (n)o: ‌
```

情報収集に同意する場合、`yes`や`y`で次に進みます。同意するとCLIの利用履歴やエラーログが収集されます。これらの情報は、サービスの品質の向上のために適切に利用させていただきます。

以降発行されたメールアドレスとパスワードでログインすることができ、正常にログインできた場合は以下のように表示されます。

```text
email: haechi@haechi.io
password: ********
🎉 Login Success from haechi@haechi.io 🎉
```

初回Henesisアカウント発行時は、仮パスワードが提供されるので、以下のコマンドを使用してパスワードを変更されることをお勧めします。

```bash
$ henesis account:changepw
```

{% hint style="info" %}
Henesis CLIはオープンソースです。ソースコードはGithubで確認することができます。 また、Henesis CLIの各種コマンドについては[README.md](https://github.com/HAECHI-LABS/henesis-cli/blob/master/README.md)に詳細に記載しています。
{% endhint %}

