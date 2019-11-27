---
description: >-
  Generates and returns an estimate of how much computation cost will be spent
  to execute the transaction.
---

# klay\_estimateComputationCost

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"klay_estimateComputationCost","params":[{"from":"0x73718c4980728857f3aa5148e9d1b471efa3a7dd", "to":"0x069942a3ca0dabf495dba872533134205764bc9c", "value":"0x0", "data":"0x2a31efc7000000000000000000000000000000000000000000000000000000000000271000000000000000000000000000000000000000000000000000000000000000420000000000000000000000000000000000000000000000000000000000003039"}, "latest"],"id":1}'
```

#### Response

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1e8b0ad"
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

`QUANTITY`: The amount of computation cost used.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction\#klay\_estimatecomputationcost](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/transaction#klay_estimatecomputationcost)

