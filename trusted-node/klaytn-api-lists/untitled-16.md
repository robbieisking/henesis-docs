---
description: Returns the current price per gas in peb.
---

# klay\_gasPrice

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_gasPrice","params":[],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x5d21dba00"
}
```

#### Parameters

None

#### Returns

`QUANTITY`: Integer of the current gas price in peb.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config\#klay\_gasprice](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config#klay_gasprice)

