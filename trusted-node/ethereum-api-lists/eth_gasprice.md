---
description: Returns the current price per gas in wei.
---

# eth\_gasPrice

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'
```

#### Response

```javascript
{
  "id":73,
  "jsonrpc": "2.0",
  "result": "0x09184e72a000" // 10000000000000
}
```

#### Parameters

none

#### Returns <a id="returns"></a>

`QUANTITY` - integer of the current gas price in wei.

#### Reference <a id="reference"></a>

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_gasprice](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gasprice)​[  
](https://docs.tn.henesis.io/ethereum/eth_estimategas)

