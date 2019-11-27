---
description: Returns the Klaytn protocol version of the node.
---

# klay\_protocolVersion

#### Request

```bash
curl https://tn.henesis.io/klaytn/<network>?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_protocolVersion","params":[""],"id":1}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "0x40"
}
```

#### Parameters

None

#### Returns

`STRING`: The Klaytn protocol version of the node.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config\#klay\_protocolversion](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/config#klay_protocolversion)



