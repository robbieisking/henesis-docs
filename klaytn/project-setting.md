# 프로젝트 세팅

## 프로젝트 생성하기

다음의 명령어를 통해 새로운 프로젝트를 만들 수 있습니다.

```bash
$ henesis init
```

이번 튜토리얼에서는 Henesis 팀에서 제공한 [템플릿](https://github.com/HAECHI-LABS/henesis-cryptokitties-klaytn)을 사용할 것입니다.  
다음의 명령어로 프로젝트 템플릿을 받아주시기 바랍니다.

```text
git clone https://github.com/HAECHI-LABS/henesis-cryptokitties-klaytn
```

## 프로젝트 구조

```text
henesis-cryptokitties-klaytn
├── backend
├── contracts
├── frontend
├── henesis.yaml
└── package.json
```

프로젝트의 전체 구조는 위와 같습니다. 튜토리얼의 백엔드 서비스는 `backend` 디렉토리에 포함되어 있으며 프론트엔드 서비스는 `frontend` 디렉토리에 포함되어 있습니다.

`contracts` 디렉토리에는 이벤 튜토리얼에서 이벤트를 구독할 컨트랙트들이 포함되어 있습니다. 그리고 `henesis.yaml`은 Henesis와 통신하기 위한 설정파일입니다.

