---
description: Returns the chain ID of the chain.
---

# klay\_chainID

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_chainID","params":[],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x2019"
}
```

#### Parameters

None

#### Returns

`QUANTITY`:Returns the chain ID of the chain.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config\#klay\_chainid](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config#klay_chainid)

