---
description: Returns the current ethereum protocol version.
---

# eth\_protocolVersion

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":67}'
```

#### Response

```javascript
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "0x54"
}
```

#### Parameters

none

#### Returns

`String` - The current ethereum protocol version.

**Reference**

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_protocolversion](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_protocolversion)



