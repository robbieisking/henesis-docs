---
description: Returns the balance of the account of given address.
---

# eth\_getBalance

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],"id":1}'
```

#### Response

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```

#### Parameters

* `DATA`, 20 Bytes - address to check for balance.
* `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter)​

#### Returns <a id="returns"></a>

`QUANTITY` - integer of the current balance in wei.

#### Reference <a id="reference"></a>

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getbalance](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getbalance)​[  
](https://docs.tn.henesis.io/ethereum/eth_gasprice)

