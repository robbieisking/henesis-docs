---
description: >-
  Executes a new message call immediately without creating a transaction on the
  block chain.
---

# eth\_call

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_call","params": [{"from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155","to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567","gas": "0x76c0","gasPrice": "0x9184e72a000","value": "0x9184e72a","data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"}, "latest"],"id":1}'
```

#### Response

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x"
}
```

#### Parameters

* `Object` - The transaction call object
  * `from`: `DATA`, 20 Bytes - \(optional\) The address the transaction is sent from.
  * `to`: `DATA`, 20 Bytes - The address the transaction is directed to.
  * `gas`: `QUANTITY` - \(optional\) Integer of the gas provided for the transaction execution. eth\_call consumes zero gas, but this parameter may be needed by some executions.
  * `gasPrice`: `QUANTITY` - \(optional\) Integer of the gasPrice used for each paid gas
  * `value`: `QUANTITY` - \(optional\) Integer of the value sent with this transaction
  * `data`: `DATA` - \(optional\) Hash of the method signature and encoded parameters. For details see [Ethereum Contract ABI](https://solidity.readthedocs.io/en/develop/abi-spec.html)
* `QUANTITY|TAG` - integer block number, or the string "latest", "earliest" or "pending", see the [default block parameter](https://github.com/ethereum/wiki/wiki/JSON-RPC#the-default-block-parameter)

#### Returns

* `DATA` - the return value of executed contract.

#### Reference

[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_call](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_call)

