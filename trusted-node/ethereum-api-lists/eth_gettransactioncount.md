---
description: Returns the number of transactions sent from an address.
---

# eth\_getTransactionCount

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f","latest"],"id":1}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

#### Parameters

* \`\`
* `DATA`, 20 Bytes - address.
* `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter)​

#### Returns <a id="returns"></a>

`QUANTITY` - integer of the number of transactions send from this address.

#### Reference <a id="reference"></a>

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_gettransactioncount](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactioncount)​[  
](https://docs.tn.henesis.io/ethereum/eth_gettransactionbyhash)

