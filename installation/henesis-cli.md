# Henesis CLI

## Henesis CLI 설치‌

Henesis CLI 도구를 사용하면 새로운 Henesis 프로젝트를 신속하게 생성하고 프로젝트 개발을 위한 명령을 실행할 수 있습니다. npm으로 Henesis CLI를 설치할 수 있습니다.

```text
$ npm install -g @haechi-labs/henesis-cli
```

위의 명령을 통해 CLI가 성공적으로 설치된 경우, 다음 명령어들을 사용하여 설치를 확인할 수 있습니다.

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

## Henesis 계정으로 로그인

{% hint style="info" %}
아직 계정을 발급 받지 않으셨다면 [hello@henesis.io](mailto:hello@henesis.io) 로 연락주세요.‌
{% endhint %}

새로운 프로젝트를 만들기 전에 먼저 CLI를 통해 로그인을 해야 합니다. 로그인은 발급된 이메일 주소와 비밀번호를 이용하여 할 수 있습니다.

```bash
$ henesis login
```

위 명령어를 실행하면 다음과 같이 정보 수집에 대한 동의를 받는 화면을 볼 수 있습니다.

```bash
Allow Henesis to collect CLI usage and error reporting information
(y)es or (n)o: ‌
```

정보 수집 여부에 `yes` 나 `y` 를 입력하여 동의하시면, CLI 사용 정보 및 오류 로그를 수집니다. 수집된 정보들은 서비스 품질 개선을 위해 소중히 사용하겠습니다.‌

이후 발급받은 이메일 주소와 비밀번호로 로그인할 수 있으며, 성공적으로 로그인하면 다음 내용이 표시됩니다.

```text
email: haechi@haechi.io
password: ********
🎉 Login Success from haechi@haechi.io 🎉
```

처음 Henesis 계정 발급시, 임시 비밀번호가 제공되니 아래 명령어를 통해 비밀번호를 변경하시는 것을 추천합니다.

```bash
$ henesis account:changepw
```

{% hint style="info" %}
Henesis CLI 는 오픈소스입니다. 해당 소스 코드는 Github에서 확인할 수 있습니다.   
Henesis CLI의 다양한 명령어는 [README.md](https://github.com/HAECHI-LABS/henesis-cli/blob/master/README.md)에서 자세히 살펴볼 수 있습니다. 
{% endhint %}

