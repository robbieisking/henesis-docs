---
description: >-
  Returns true if an input account has a non-empty codeHash at the time of a
  specific block number. It returns false if the account is an EOA or a smart
  contract account which doesn't have codeHash.
---

# klay\_isContractAccount

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_isContractAccount","params":["0x2f07d5b3fa1051460099dc9ea0c2975b6ea67776", "latest"],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": false
}
```

#### Parameters

* `20-byte DATA`: Address to check for balance.
* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`Boolean`: `true` means the input parameter is an existing smart contract address.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/account\#klay\_iscontractaccount](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/account#klay_iscontractaccount)

