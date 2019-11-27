---
description: Returns the unit price of the given block in peb.
---

# klay\_gasPriceAt

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_gasPriceAt","params":["0x64"],"id":1}'
```

#### Response

```javascript
{
  "jsonrpc": "2.0",
  "id":1,
  "result": "0x5d21dba00" // 25,000,000,000 peb = 25 Gpeb
}
```

#### Parameters

None

#### Returns

`QUANTITY`: Integer of the current gas price in peb.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config\#klay\_gaspriceat](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config#klay_gaspriceat)



