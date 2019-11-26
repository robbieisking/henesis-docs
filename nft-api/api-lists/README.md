# API Lists

Henesis NFT APIは以下のようなAPIをサポートします。

* `getTokensByAccountAddress` : 該当Accountが所有しているNFTのリストを表示します。
* `getContract` : ERC721標準を満足するコントラクトの情報を表示します。
* `getTokensByContractAddress` : トークンコントラクトにNFTのリストを取得します。
* `getAllContracts` : ERC721基準を満たしているすべてのコントラクトの情報を表示します。

{% hint style="info" %}
NFT APIはEvent Streamerを利用して実装されています。したがって、特定のthreshold以降confirmされたブロックに含まれている情報を提供するため、現在のブロック番号との時間差が存在することができます。
{% endhint %}

