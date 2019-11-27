---
description: Returns the balance of the account of given address.
---

# klay\_getBalance

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x0"
}
```

#### Parameters

* `20-byte DATA`: Address to check for balance.
* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`QUANTITY`: Integer of the current balance in peb.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/account\#klay\_getbalance](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/account#klay_getbalance)
