---
description: >-
  Returns the size of the council at the specified block. If the parameter is
  not set, returns the size of the council at the latest block.
---

# klay\_getCouncilSize

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0", "method":"klay_getCouncilSize", "params":["0x1b4"],"id":73}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 73,
    "result": 19
}
```

#### Parameters

* `QUANTITY | TAG`: \(optional\) Integer of a block number, or the string `"earliest"` or `"latest"`.

#### Returns

`Integer:`The size of the council, or `-1` when no council was found.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getcouncilsize](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getcouncilsize)

