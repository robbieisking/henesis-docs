# 실행하기

## Dependency 설치하기

{% code-tabs %}
{% code-tabs-item title="project root에서 " %}
```bash
npm run installAll
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 프론트엔드와 백엔드 실행하기 

{% code-tabs %}
{% code-tabs-item title="./frontend" %}
```bash
npm run start
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="./backend" %}
```bash
npm run dev
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 확인하기

위의 명령어를 입력한다면 아래와 같이 실행 되는지 확인해주세요.

#### 프론트엔드 

{% code-tabs %}
{% code-tabs-item title="Bash" %}
```bash
Compiled successfully!

You can now view app in the browser.

  Local:            http://localhost:3000/
  On Your Network:  http://192.168.0.41:3000/

Note that the development build is not optimized.
To create a production build, use yarn build.
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Web Browser, &#xC544;&#xC9C1; &#xAD6C;&#xB3C5;&#xD55C; &#xC774;&#xBCA4;&#xD2B8;&#xAC00; &#xC5C6;&#xC744; &#xB54C;](../../.gitbook/assets/image.png)

![Web Browser, &#xAD6C;&#xB3C5;&#xB41C; &#xC774;&#xBCA4;&#xD2B8;&#xAC00; &#xC788;&#xC744; &#xB54C;](../../.gitbook/assets/image%20%282%29.png)

#### 백엔드 

{% code-tabs %}
{% code-tabs-item title="Bassh" %}
```bash
MacBook-Pro-10:backend henesis$ npm run dev

> express-es6-rest-api@0.3.0 dev /Users/henesis/tutorial/backend
> nodemon -w src --exec "babel-node src --presets es2015,stage-0"

[nodemon] 1.19.2
[nodemon] to restart at any time, enter `rs`
[nodemon] watching dir(s): src/**/*
[nodemon] starting `babel-node src --presets es2015,stage-0`
Started on port 8080
```
{% endcode-tabs-item %}
{% endcode-tabs %}

