---
description: Returns code at a given address.
---

# klay\_getCode

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'
```

#### Response

```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result":   "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```

#### Parameters

* `20-byte DATA`: Address to check for balance.
* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`QUANTITY`: Integer of the current balance in peb.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/account\#klay\_getcode](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/account#klay_getcode)

