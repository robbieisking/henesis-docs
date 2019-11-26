---
description: 트랜잭션의 상태 변경을 Henesis를 통해 쉽게 추적할 수 있습니다.
---

# Introduction

## 트랜잭션이란?

트랜잭션이란 블록체인에 기록된 상태를 바꾸기 위해 계정\(account\)이 데이터를 전자 서명한 후 보내는 것입니다.

블록체인에 상태를 변경하기 위해 트랜잭션을 만들어 블록체엔 네트워크에 전파해야 합니다. 하지만 블록체인에서 트랜잭션을 다루기는 어렵습니다. 왜 그럴까요? 

* 트랜잭션은 실시간으로 블록에 담기지 않습니다. 따라서 유저에게 정확한 정보를 주기 위해서는 트랜잭션의 현재 상태를 정확히 파악할 수 있어야 합니다. 
* 상황에 따라서 트랜잭션 채굴이 늦어지거나 이뤄지지 않을 수 있습니다.

즉, 트랜잭션을 제대로 다룬다는 것은 다음을 의미합니다. 

* 트랜잭션이 최종적으로 채굴되었는지 지속적으로 확인하고 그 결과를 유저에게 알려줍니다. 
* 트랜잭션이 채굴되지 않았다면 원인을 파악하고 적절한 조치를 취합니다.

한마디로 트랜잭션의 상태를 실시간으로 모니터링 해서, 해당 상태에 적절한 행동을 취해야 합니다. 그렇다면 트랜잭션은 어떠한 상태가 있을 수 있을까요?

## 트랜잭션의 상태

Henesis에서는 트랜잭션의 상태를 크게 **3가지**로 구분합니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Status</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Pending</b>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>&#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xC744; &#xC0DD;&#xC131;&#xD588;&#xC73C;&#xB098;
          &#xC544;&#xC9C1; &#xCC44;&#xAD74;&#xB418;&#xC9C0; &#xC54A;&#xC740; &#xC0C1;&#xD669;&#xC785;&#xB2C8;&#xB2E4;.
          &#xC774;&#xB7F0; &#xACBD;&#xC6B0;&#xB294; &#xD06C;&#xAC8C; &#xB450; &#xAC00;&#xC9C0;
          &#xC0C1;&#xD669;&#xC5D0; &#xC758;&#xD574; &#xBC1C;&#xC0DD;&#xD560; &#xC218;
          &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.</p>
        <ol>
          <li>&#xBE14;&#xB85D;&#xCCB4;&#xC778; &#xB124;&#xD2B8;&#xC6CC;&#xD06C;&#xC5D0;
            &#xC62C;&#xBC14;&#xB974;&#xAC8C; &#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xC774;
            &#xC804;&#xD30C;&#xB418;&#xC9C0; &#xC54A;&#xC544;&#xC11C; &#xCC44;&#xAD74;&#xC790;
            &#xB178;&#xB4DC;&#xC758; txpool&#xC5D0; &#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xC774;
            &#xB2F4;&#xAE30;&#xC9C0; &#xC54A;&#xC740; &#xACBD;&#xC6B0;</li>
          <li>Gas price&#xAC00; &#xB0AE;&#xC544;&#xC11C; &#xCC44;&#xAD74;&#xC790;&#xB4E4;&#xC774;
            &#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xC744; &#xBE14;&#xB85D;&#xC5D0; &#xB2F4;&#xC9C0;
            &#xC54A;&#xC744; &#xACBD;&#xC6B0;</li>
        </ol>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Receipt</b>
      </td>
      <td style="text-align:left">&#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xC774; &#xCC44;&#xAD74;&#xB418;&#xC11C;
        &#xBE14;&#xB85D;&#xC5D0; &#xB2F4;&#xAE34; &#xC0C1;&#xD0DC;&#xC785;&#xB2C8;&#xB2E4;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Confirmation</b>
      </td>
      <td style="text-align:left">&#xD2B8;&#xB79C;&#xC7AD;&#xC158;&#xC774; &#xCC44;&#xAD74;&#xB41C; &#xD6C4;
        &#xCD94;&#xAC00;&#xC801;&#xC73C;&#xB85C; &#xBE14;&#xB85D;&#xC774; &#xB354;
        &#xC0DD;&#xACA8;&#xC11C; Chain Reorganization&#xC774; &#xC77C;&#xC5B4;&#xB098;&#xC9C0;
        &#xC54A;&#xC744; &#xC0C1;&#xD0DC;&#xC785;&#xB2C8;&#xB2E4;.</td>
    </tr>
  </tbody>
</table>## 트랜잭션 상태 추적의 어려움 

트랜잭션에는 위와 같은 상태가 있다는 것을 알았습니다. 하지만 서비스에서 발생하는 모든 트랜잭션의 상태를 추적하는 것은 생각보다 어렵습니다.

* 추적하는 트랜잭션 양이 많아질수록 시스템 리소스를 많이 소비합니다.
* `Pending`상태를 제대로 파악하기 위해서는 트랜잭션 풀을 조회해야 합니다. 하지만 대부분의 노드 제공자들은 트랜잭션 풀을 조회하기 위한 RPC 호출을 열어두지 않습니다. 따라서 직접 노드를 운영해야 합니다.
* 트랜잭션 상태가 갱신될 때 마다 실시간으로 알려주는 인프라 구축이 필요합니다.

## Henesis는 위의 어려움을 다음과 같이 해결합니다

* 트랜잭션 상태 추적 management service를 제공하여 시스템 리소스를 확장하지 않아도 많은 트랜잭션을 다룰 수 있습니다. 
* Henesis의 블록체인 노드를 이용하기에 트랜잭션 풀을 조회할 수 있고, 직접 노드를 운영할 필요가 없습니다.
* Websocket을 이용해 트랜잭션의 상태를 실시간으로 알려주기 때문에, 메세지 전달을 위한 별도의 인프라 구축이 필요 없습니다. 







