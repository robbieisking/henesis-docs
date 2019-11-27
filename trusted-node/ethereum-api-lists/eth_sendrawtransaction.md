---
description: >-
  Creates new message call transaction or a contract creation for signed
  transactions.
---

# eth\_sendRawTransaction

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":[{see above}],"id":1}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

#### Parameters

* `DATA`, The signed transaction data.

#### Returns

`DATA`, 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

Use [eth\_getTransactionReceipt](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

#### Reference

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_sendrawtransaction](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sendrawtransaction)\`\`

