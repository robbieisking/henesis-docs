---
description: >-
  Returns a list of all validators of the council at the specified block. If the
  parameter is not set, returns a list of all validators of the council at the
  latest block.
---

# klay\_getCouncil

#### Request

```bash
curl https://tn.henesis.io/klaytn/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0", "method":"klay_getCouncil", "params":["0x1b4"],"id":73}'
```

#### Response

```javascript
{
    "jsonrpc": "2.0",
    "id": 73,
    "result": [
        "0x0b59cae1f03534209fdb9ddf5ea65b310cd7060c",
        "0x16c192585a0ab24b552783b4bf7d8dc9f6855c35",
        "0x1782834bf8847e235f21f2c1f13fca4d5dff6621",
        "0x2bdd279522b8a0843831fbb94cfbb24a913597c5",
        "0x386ca3cb8bb13f48d1a6adc1fb8df09e7bb7f9c8",
        "0x45ed4bf508ca2061a8b4a16e278ea85a8f87a70b",
        "0x52d41ca72af615a1ac3301b0a93efa222ecc7541",
        "0x56e3a565e31f8fb0ba0b12c03355518c64372120",
        "0x56e8c1463c341abf8b168c3079ea41ce8a387e18",
        "0xa2ba8f7798649a778a1fd66d3035904949fec555",
        "0xbca8ffa45cc8e30bbc0522cdf1a1e0ebf540dfe2",
        "0xe3d92072d8b9a59a0427485a1b5f459271df457c",
        "0xe783fc94fddaeebef7293d6c5864cff280f121e1",
        "0xe93a890fb7ec5e993b1a7fd77b0d13a0763eff3d",
        "0xec6c1cede510be308f0fdbbc8dbdf238829bdb34",
        "0xed6ee8a1877f9582858dbe2509abb0ac33e5f24e",
        "0xf113ec8c22765d485309cf1d025d1b975245b9f8",
        "0xf6623192aa096e192024ecb381b04c58df9a6075",
        "0xf8c9c61c5e7f2b6219d1c28b94e5cb3cdc802594"
    ]
}
```

#### Parameters

* `QUANTITY | TAG`: \(Optional\) Integer block number, or the string `"latest"`, `"earliest"` 

#### Returns

`Array of 20-byte DATA`: Addresses of all validators of the council.

**Reference**

[https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block\#klay\_getcouncil](https://docs.klaytn.com/bapp/json-rpc/api-references/klay/block#klay_getcouncil)

