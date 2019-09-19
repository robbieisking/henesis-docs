# Henesis Project



이번 장에서는 스마트 컨트랙트 이벤트 데이터를 읽어오기 위한 henesis 설정 방법을 안내합니다. 듣고 싶은 스마트 컨트랙트에 따라 `contracts/` 디렉토리와 `henesis.yaml` 를 알맞게 수정하면 해당 스마 컨트랙트에서 발생하는 이벤트 정보를 구독할 수 있습니다.

본 예제에서는 블록체인에서 가장 많은 유저가 사용했던 [크립토키티](https://www.cryptokitties.co/) 스마트 컨트랙트를 토대로 어떻게 스마트 컨트랙트와 Henesis를 연동하는지를 설명합니다.

사용자들이 크립토키티 게임을 하면서 스마트 컨트랙트로 트랜잭션을 보낼때, 크립토키티 스마트 컨트랙트에서는 이벤트가 발생하며, Henesis는 해당 이벤트 데이터들을 빠짐없이 구독할 수 있도록 도와줍니다.

{% hint style="info" %}
크립토키티는 이더리움 블록체인에서 ERC-721을 이용하여 어플리케이션을 만든 첫번째 사례로 알려져 있습니다. ERC-721은 NFT\(Non-Fungible Token\)의 표준 인터페이스이며, 구체적인 사양은 다음 [제안서](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md)에서 확인할 수 있습니다.
{% endhint %}

