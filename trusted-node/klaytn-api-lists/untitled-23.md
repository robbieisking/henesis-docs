---
description: >-
  Creates a new message call transaction or a contract creation for signed
  transactions.
---

# klay\_sendRawTransaction

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_sendRawTransaction","params":["your-signed-tx"],"id":1}'
```

#### Response

```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

#### Parameters

* `DATA`: The signed transaction data.

**Returns**

`32-byte DATA`: The transaction hash or the zero hash if the transaction is not yet available.

