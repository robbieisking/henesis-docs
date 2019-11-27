---
description: Returns the receipt of a transaction by transaction hash.
---

# eth\_getTransactionReceipt

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'
```

#### Response

```javascript
{
    "id":1,
    "jsonrpc":"2.0",
    "result": {
        "transactionHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
        "transactionIndex":  "0x1", // 1
        "blockNumber": "0xb", // 11
        "blockHash": "0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b",
        "cumulativeGasUsed": "0x33bc", // 13244
        "gasUsed": "0x4dc", // 1244
        "contractAddress": "0xb60e8dd61c5d32be8058bb8eb970870f07233155", // or null, if none was created
        "logs": [{ 
            // logs as returned by getFilterLogs, etc.
        }, ...],
        "logsBloom": "0x00...0", // 256 byte bloom filter
        "status": "0x1"
    }
}
```

#### Parameters

* `DATA`, 32 Bytes - hash of a transaction

**Returns**

`Object` - A transaction receipt object, or `null` when no receipt was found

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">transactionHash</td>
      <td style="text-align:left">32-byte DATA</td>
      <td style="text-align:left">Hash of the transaction.</td>
    </tr>
    <tr>
      <td style="text-align:left">transactionIndex</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">Integer of the transaction index position in the block.</td>
    </tr>
    <tr>
      <td style="text-align:left">blockHash</td>
      <td style="text-align:left">
        <p>32-byte</p>
        <p>DATA</p>
      </td>
      <td style="text-align:left">hash of the block where this transaction was in.</td>
    </tr>
    <tr>
      <td style="text-align:left">blockNumber</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">block number where this transaction was in.</td>
    </tr>
    <tr>
      <td style="text-align:left">from</td>
      <td style="text-align:left">
        <p>20-byte</p>
        <p>DATA</p>
      </td>
      <td style="text-align:left">address of the sender</td>
    </tr>
    <tr>
      <td style="text-align:left">to</td>
      <td style="text-align:left">
        <p>20-byte</p>
        <p>DATA</p>
      </td>
      <td style="text-align:left">address of the receiver. null when it&apos;s a contract creation transaction.</td>
    </tr>
    <tr>
      <td style="text-align:left">cumulativeGasUsed</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">The total amount of gas used when this transaction was executed in the
        block.</td>
    </tr>
    <tr>
      <td style="text-align:left">gasUsed</td>
      <td style="text-align:left">
        <p>&#x200B;</p>
        <p>QUANTITY</p>
        <p>&#x200B;</p>
      </td>
      <td style="text-align:left">The amount of gas used by this specific transaction alone.</td>
    </tr>
    <tr>
      <td style="text-align:left">contractAddress</td>
      <td style="text-align:left">20-byte DATA</td>
      <td style="text-align:left">The contract address created, if the transaction was a contract creation,
        otherwise null</td>
    </tr>
    <tr>
      <td style="text-align:left">logs</td>
      <td style="text-align:left">Array</td>
      <td style="text-align:left">Array of log objects, which this transaction generated.</td>
    </tr>
    <tr>
      <td style="text-align:left">logsBloom</td>
      <td style="text-align:left">256-byte DATA</td>
      <td style="text-align:left">Bloom filter for light clients to quickly retrieve related logs.</td>
    </tr>
  </tbody>
</table>It also returns _either_ :

* `root` : `DATA` 32 bytes of post-transaction stateroot \(pre Byzantium\)
* `status`: `QUANTITY` either `1` \(success\) or `0` \(failure\)

**Reference**

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_gettransactionreceipt](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactionreceipt)​[  
](https://docs.tn.henesis.io/ethereum/eth_gettransactioncount)

