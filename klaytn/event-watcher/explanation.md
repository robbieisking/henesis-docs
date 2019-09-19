# Cryptokitties Event Watcher 설명

## 서비스 흐름

튜토리얼에서 이용되는 예제 서비스는 다음과 같은 흐름을 가지고 동작합니다.

1. 백엔드에서 [**Henesis-SDK**](https://github.com/HAECHI-LABS/henesis-sdk-js)를 이용하여 Henesis를 통해 스마트 컨트랙트의 이벤트를 구독합니다.
2. 이벤트가 전달되면 백엔드는 모델에 정보를 저장합니다.
3. 프론트엔드가 백엔드로부터 event를 polling하여 화면에 보여줍니다.

#### 프론트엔드 

{% code-tabs %}
{% code-tabs-item title="./frontend/src" %}
```text
src
├── App.css
├── App.js
├── App.test.js
├── components
│   ├── CustomChip.js: event parameter를 chip으로 표현.
│   └── EventRows.js: event들을 여려개의 TableRow로 표현.
├── containers
│   ├── EventList.js: 이벤트 list를 보여주는 container.
│   └── MetaData.js: 블록체인 정보, 컨트랙트의 주소 등을 보여주는 container.
├── henesis.png
├── index.css
├── index.js
└── serviceWorker.js
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### 백엔드

{% code-tabs %}
{% code-tabs-item title="./backend/src" %}
```text
src
├── api.js: RESTful api router.
├── config.json: [구현 필요] 여기에서 integrationId를 변경해야합니다. 
├── event.js: [구현 필요] henesis sdk를 이용해 스마트 컨트랙트의 event를 구독, 모델에 저장합니다.
├── index.js: 프로그램 실행지점.
└── model.js: event 객체의 모델.
```
{% endcode-tabs-item %}
{% endcode-tabs %}

샘플 레포지토리 master 브랜치의 코드는 Henesis로부터 이벤트를 받아오는 코드를 비워놓은 상태입니다. 이번 챕터의 목표는 이벤트를 받아오는 부분을 직접 구현해보면서 Henesis와 어떻게 통신하는지 익혀보는 것입니다.

이번 튜토리얼에서 **\[구현 필요\]** 라고 표시된 부분을 작성해 코드를 완성해보겠습니다.

## 해야할 일

```typescript
import { Router } from 'express';
import resource from 'resource-router-middleware';

const events = ({ config, model }) => resource({
	id : 'event',
	index({ params }, res) {
		res.json(model.getEvents());
	}
});

export default ({ config, model }) => {
	let api = Router();

	// mount the event resource
	api.use('/events', events({ config, model }));

	...

	return api;
}
```

예제 코드는 현재 Henesis 통해 데이터를 받아오는 코드가 비워져 있습니다. 해당 부분을 채워서 Henesis 사용법을 익히는 것이 이번 챕터의 주요 목적입니다.

백엔드는 앞서 배포한 Integration을 활용해 크립토키티 스마트 컨트랙트에서 발생한 이벤트를 저장하고, 저장된 이벤트를 API를 통해 조회할  수 있습니다.

프론트엔드는 백엔드의 `/api/events` API를 통해 이벤트를 polling 후 화면으로 보여줍니다. 

이번에 구현해야할 부분은

1. WebSocket을 이용하여 Henesis 로 부터 이벤트를 구독한다.
2. 구독한 이벤트를 모델에 저장한다.

두 가지이며, 이를 `./backend/src/event.js`에 구현합니다.

