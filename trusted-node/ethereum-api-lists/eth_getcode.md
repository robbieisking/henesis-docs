---
description: Returns code at a given address.
---

# eth\_getCode

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```

#### Parameters

* `DATA`, 20 Bytes - address.
* `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter).

#### Returns

`DATA` - the code from the given address.

#### Reference

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getcode](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getcode)

