---
description: >-
  Executes a new message call immediately without creating a transaction on the
  block chain. It returns data or an error object of JSON RPC if error occurs.
---

# klay\_call

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc": "2.0", "method": "klay_call", "params": [{"from": "0x3f71029af4e252b25b9ab999f77182f0cd3bc085", "to": "0x87ac99835e67168d4f9a40580f8f5c33550ba88b", "gas": "0x100000", "gasPrice": "0x5d21dba00", "value": "0x0", "data": "0x8ada066e"}, "latest"], "id": 1}'
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

* `Object` : The transaction `callObject`.
  * from\(`20-byte DATA`\): \(optional\) The address the transaction is sent from.
  * to\(`20-byte DATA`\): The address the transaction is directed to.
  * gas\(`QUANTITY`\):  \(optional\) Integer of the gas provided for the transaction execution. klay\_call consumes zero gas, but this parameter may be needed by some executions.
  * gasPrice\(`QUANTITY`\): \(optional\) Integer of the gasPrice used for each paid gas.
  * value\(`QUANTITY`\): \(optional\) Integer of the value sent with this transaction.
  * data\(`DATA`\): \(optional\) Hash of the method signature and encoded parameters.
* `QUANTITY | TAG`: Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`

#### Returns

`DATA` : The return value of executed contract.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction\#klay\_call](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction#klay_call)

