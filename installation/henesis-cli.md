# Henesis CLI

## Installation <a id="installation"></a>

Through Henesis CLI, you can generate a new project and execute a command for developing DApps. You can install the Henesis CLI via "npm".

```text
$ npm install -g @haechi-labs/henesis-cli
```

* Check that it has been installed successfully by running `henesis`

```text
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

## Login <a id="login"></a>

{% hint style="info" %}
Have you requested us to issue an account to use the Henesis?  
If you haven't yet, please contact [hello@henesis.io](mailto:hello@henesis.io).
{% endhint %}

Before you create a new project, you will need to login through our CLI. You can login with the email and password which we issued.

```text
$ henesis login
```

After you execute the above command, you can see a screen asking for consent to collect information as follows.

```text
Allow Henesis to collect CLI usage and error reporting information
yes(y) or no(n):
```

{% hint style="info" %}
If you enter 'yes' or 'y' in the CLI, you consent to our collection of your usage information of CLI and error logs. The information we collect will only be used to improve the quality of service.
{% endhint %}

If you enter your email and password successfully, the following contents will show.

```text
email: hello@henesis.io
password: ******
🎉 Login Success from hello@henesis.io 🎉
```

It is recommended to change your initial password through the below command.

```text
$ henesis account:changepw
```

{% hint style="info" %}
Henesis CLI is an open-source project. You can see the repository in the Github. Also, you can see the other commands in the [README.md](https://github.com/HAECHI-LABS/henesis-cli/blob/master/README.md) ​
{% endhint %}



