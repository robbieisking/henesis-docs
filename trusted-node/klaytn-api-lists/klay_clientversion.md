---
description: Returns the current client version of a Klaytn node.
---

# klay\_clientVersion

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_clientVersion", "params":[],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "Klaytn/v1.1.1/linux-amd64/go1.12.5"
}
```

#### Parameters

None

#### Returns

`STRING`: The current client version of a Klaytn node.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config\#klay\_clientversion](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config#klay_clientversion)

