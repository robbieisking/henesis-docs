# 크립토키티 스마트 컨트랙트

본 튜토리얼에서는 NFT를 이용한 블록체인 게임인 크립토키티 스마트 컨트랙트를 예제로 설명합니다. 크립토키티 스마트 컨트랙트는 클레이튼 바오밥 네트워크에 이미 배포되어 있으며, 각각의 스마트 컨트랙트 주소는 다음과 같습니다.‌

* KittyCore : [0x20A1CFF753ae36988f73507A5497f02369970D70](https://baobab.scope.klaytn.com/account/0x20A1CFF753ae36988f73507A5497f02369970D70)
* SaleClockAuction : [0xD1dFd8F3bd2f05D497296Fc1C44Ff8b397189a03](https://baobab.scope.klaytn.com/account/0xD1dFd8F3bd2f05D497296Fc1C44Ff8b397189a03)
* SiringClockAuction : [0xC1C88A7B90062866F54320b18Ee5DA65F79202D6](https://baobab.scope.klaytn.com/account/0xC1C88A7B90062866F54320b18Ee5DA65F79202D6)

## 이벤트 <a id="undefined"></a>

크립토키티 스마트 컨트랙트에는 아래와 같이 많은 이벤트들이 정의되어 있습니다. Henesis는 지정한 스마트 컨트랙트에서 발생하는 모든 이벤트를 전달해줍니다.

```text
event Transfer(address from, address to, uint256 tokenId);
event Approval(address owner, address approved, uint256 tokenId);
event Pregnant(address owner, uint256 matronId, uint256 sireId, uint256 cooldownEndBlock);
event Birth(address owner, uint256 kittyId, uint256 matronId, uint256 sireId, uint256 genes);
event ContractUpgrade(address newContract);
event AuctionCreated(uint256 tokenId, uint256 startingPrice, uint256 endingPrice, uint256 duration);
event AuctionSuccessful(uint256 tokenId, uint256 totalPrice, address winner);
event AuctionCancelled(uint256 tokenId);
event Pause();
event Unpause();
```

* Transfer
  * 한 주소에서 다른 주소로의 토큰 전송을 나타냄
  * `from`: 보내는 사람의 이더리움 주소
  * `to`: 받는 사람의 이더리움 주소
  * `tokenId`: 전송할 ERC-721 토큰의 ID
* Approval
  * 한 주소에서 다른 주소로 토큰 전송 권한에 대한 위임을 나타냄
  * `owner`: 토큰 소유자의 이더리움 주소
  * `approved`: 전송 권한 수임자의 이더리움 주소
  * `tokenId`: 대상 ERC-721 토큰의 ID
* Pregnant
  * 교배를 통해 새끼고양이를 임신했음을 나타
  * `owner`: 고양이 소유자의 이더리움 주소
  * `matronId`: 엄마 고양이의 아이디
  * `sireId`: 아빠 고양이의 아이디
  * `cooldownEndBlock`: 다음 번 교배가 가능한 시간
* Birth
  * 새로운 고양이가 태어났음을 나타
  * `owner`: 고양이 소유자의 이더리움 주소
  * `kittyId`: 새끼 고양이의 아이디
  * `matronId`: 엄마 고양이의 아이디
  * `sireId`: 아빠 고양이의 아이디
  * `genes`: 새끼 고양이의 유전자 정보
* ContractUpgrade
  * 크립토키티 스마트 컨트랙트의 업그레이드를 나타냄
  * `newContract`: 새로운 스마트 컨트랙트의 이더리움 주소
* AuctionCreated
  * 새로운 옥션이 생성되었음을 나타냄
  * `tokenId`: 대상 ERC-721 토큰의 ID
  * `startingPrice`: 경매 시작가
  * `endingPrice`: 경매 종료가
  * `duration`: 경매 시간
* AuctionSuccessful
  * 옥션이 성공적으로 마무리 되었음을 나타냄
  * `tokenId`: 대상 ERC-721 토큰의 ID
  * `totalPrice`: 낙찰 가격
  * `winner`: 옥션을 통해 토큰을 획득한 계정의 이더리움 주소
* AuctionCancelled
  * 옥션이 취소 되었음을 나타냄
  * `tokenId`: 대상 ERC-721 토큰의 ID
* Pause
  * 크립토키티 스마트 컨트랙트의 일시정지를 나타냄
* Unpause
  * 일시정지의 해제를 나타냄

## 솔리디티 파일 작성하기 <a id="undefined-1"></a>

Henesis는 스마트 컨트랙트로부터 이벤트들을 구독하기 위해서 스마트 컨트랙트 소스코드가 필요합니다. 본 튜토리얼에서는 크립토키티 컨트랙트의 소스코드를 사용하고 있으며, `contracts/` 폴더 내에서 확인하실 수 있습니다.

```text
contracts/ 
├── KittyCore.sol 
├── SaleClockAuction.sol 
└── SiringClockAuction.sol‌
```

* KittyCore: 메인 로직을 담고 있는 스마트 컨트랙트이며 고양이 데이터와 소유권 정보 저장됩니다. 또한 교배, 교환 및 경매와 같은 주요 작업에 대한 스마트 컨트랙트입니다.
* SaleClockAuction: 사용자가 0세 새끼 고양이를 얻을수 있는 곳이며 누구나 새끼 고양이를 경매에 게시 수있는 마켓 플레이스입니다.
* SiringClockAuction: 교배를 위한 옥션 마켓 플레이스입니다. 한 유저가 교배를 위해 옥션에 자신의 고양이 등록하면 다른 참여자들이 옥션에 참여하여 교배를 할 수 있습니다.

'contracts/' 디렉토리에 스마트 컨트랙트 전체 소스 코드를 복사 & 붙여넣기로 추가할 수 있습니다.

하지만 Henesis는 이벤트를 구독하는데에는 오직 [Application Binary Interface\(ABI\)](https://solidity.readthedocs.io/en/v0.5.3/abi-spec.html) 만이 필요하기 때문에 구독하고자 하는 이벤트들과 함수들에 대해서만 명시하여도 이벤트를 구독할 수 있습니다.[  
](https://app.gitbook.com/@henesis/s/henesis-1/~/drafts/-Lo42t5JBukM_22stag0/primary/undefined-3/untitled)

