# eth\_getBlockByHash

#### Request

```bash
curl https://tn.henesis.io/ethereum/mainnet?clientId=<clientId> \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0x375c6be8ee2c0e6d9c9bd64aea4b0da724eb9883ef45a96e7bb1f90d50a3cc82", true],"id":1}'
```

#### Response

```javascript
{
    "id":1,
    "jsonrpc":"2.0",
    "result": {
        "number": "0x1b4", // 436
        "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        "parentHash": "0x9646252be9520f6e71339a8df9c55e4d7619deeb018d2a3f2d21fc165dde5eb5",
        "nonce": "0xe04d296d2460cfb8472af2c5fd05b5a214109c25688d3704aed5484f9a7792f2",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
        "miner": "0x4e65fda2159562a496f9f3522f89122a3088497a",
        "difficulty": "0x027f07", // 163591
        "totalDifficulty":  "0x027f07", // 163591
        "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "size":  "0x027f07", // 163591
        "gasLimit": "0x9f759", // 653145
        "gasUsed": "0x9f759", // 653145
        "timestamp": "0x54e34e8e" // 1424182926
        "transactions": [{...},{ ... }] 
        "uncles": ["0x1606e5...", "0xd5145a9..."]
    }
}

```

#### Parameters

* `DATA`, 32 Bytes - Hash of a block.
* `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions.

#### Returns <a id="returns"></a>

`Object` - A block object, or `null` when no block was found:

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
      <td style="text-align:left">number</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">The block number. <code>null</code> when it is pending block.</td>
    </tr>
    <tr>
      <td style="text-align:left">hash</td>
      <td style="text-align:left">32-byte DATA</td>
      <td style="text-align:left">Hash of the block. <code>null</code> when it is pending block.</td>
    </tr>
    <tr>
      <td style="text-align:left">parentHash</td>
      <td style="text-align:left">32-byte DATA</td>
      <td style="text-align:left">Hash of the parent block.</td>
    </tr>
    <tr>
      <td style="text-align:left">logsBloom</td>
      <td style="text-align:left">256-byte DATA</td>
      <td style="text-align:left">The bloom filter for the logs of the block. <code>null</code> when it is
        pending block.</td>
    </tr>
    <tr>
      <td style="text-align:left">nonce</td>
      <td style="text-align:left">
        <p>8-byte</p>
        <p>DATA</p>
      </td>
      <td style="text-align:left">Hash of the generated proof-of-work. null when its pending block.</td>
    </tr>
    <tr>
      <td style="text-align:left">sha3Uncles</td>
      <td style="text-align:left">DATA</td>
      <td style="text-align:left">SHA3 of the uncles data in the block.</td>
    </tr>
    <tr>
      <td style="text-align:left">transactionsRoot</td>
      <td style="text-align:left">32-byte DATA</td>
      <td style="text-align:left">The root of the transaction trie of the block.</td>
    </tr>
    <tr>
      <td style="text-align:left">stateRoot</td>
      <td style="text-align:left">32-byte DATA</td>
      <td style="text-align:left">The root of the final state trie of the block.</td>
    </tr>
    <tr>
      <td style="text-align:left">receiptsRoot</td>
      <td style="text-align:left">32-byte DATA</td>
      <td style="text-align:left">The root of the receipts trie of the block.</td>
    </tr>
    <tr>
      <td style="text-align:left">reward</td>
      <td style="text-align:left">20-byte DATA</td>
      <td style="text-align:left">The address of the beneficiary to whom the block rewards were given.</td>
    </tr>
    <tr>
      <td style="text-align:left">miner</td>
      <td style="text-align:left">
        <p>20-byte</p>
        <p>DATA</p>
      </td>
      <td style="text-align:left">The address of the beneficiary to whom the mining rewards were given.</td>
    </tr>
    <tr>
      <td style="text-align:left">difficulty</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">integer of the difficulty for this block.</td>
    </tr>
    <tr>
      <td style="text-align:left">totalDifficulty</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">integer of the total difficulty of the chain until this block.</td>
    </tr>
    <tr>
      <td style="text-align:left">extraData</td>
      <td style="text-align:left">DATA</td>
      <td style="text-align:left">The &quot;extra data&quot; field of this block.</td>
    </tr>
    <tr>
      <td style="text-align:left">size</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">Integer the size of this block in bytes.</td>
    </tr>
    <tr>
      <td style="text-align:left">gasLimit</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">The maximum gas allowed in this block.</td>
    </tr>
    <tr>
      <td style="text-align:left">gasUsed</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">The total used gas by all transactions in this block.</td>
    </tr>
    <tr>
      <td style="text-align:left">timestamp</td>
      <td style="text-align:left">QUANTITY</td>
      <td style="text-align:left">The Unix timestamp for when the block was collated.</td>
    </tr>
    <tr>
      <td style="text-align:left">transactions</td>
      <td style="text-align:left">Array</td>
      <td style="text-align:left">Array of transaction objects, or 32-byte transaction hashes depending
        on the last given parameter.</td>
    </tr>
    <tr>
      <td style="text-align:left">uncles</td>
      <td style="text-align:left">Array</td>
      <td style="text-align:left">Array of uncle hashes.</td>
    </tr>
  </tbody>
</table>**Reference**

​[https://github.com/ethereum/wiki/wiki/JSON-RPC\#eth\_getblockbyhash](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getblockbyhash)​[  
](https://docs.tn.henesis.io/ethereum/eth_getbalance)

