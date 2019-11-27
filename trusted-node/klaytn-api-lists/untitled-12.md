---
description: >-
  Returns the size of the committee at the specified block. If the parameter is
  not set, returns the size of the committee at the latest block.
---

# klay\_getCommitteeSize

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0", "method":"klay_getCommitteeSize", "params":["0x1b4"],"id":73}'
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

* `QUANTITY | TAG`: \(Optional\) Integer block number, or the string `"latest"`, `"earliest"` 

#### Returns

`Integer` - The size of the committee, or `-1` when no committee was found.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getcommitteesize](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getcommitteesize)

