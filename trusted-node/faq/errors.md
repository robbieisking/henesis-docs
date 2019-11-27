# Errors

### Authentication Failure <a id="authentication-failure"></a>

If the wrong client ID is used, the following error message is shown.

```javascript
// http status: 401
{
    "jsonrpc": "2.0",
    "id": 1,
    "error": "authentication error"    
} 
```

### Unsupported JSON RPC <a id="unsupported-json-rpc"></a>

```javascript
// http status: 403
{
    "error": "unsupported json rpc method",
    "id": 1,
    "jsonrpc": "2.0"
}
```

### Internal Server Error <a id="internal-server-error"></a>

```javascript
// http status: 500
{
    "error": "internal error",
    "id": 1,
    "jsonrpc": "2.0"
}
```

​[Ethereum JSON RPC Standard errors](https://github.com/ethereum/wiki/wiki/JSON-RPC-Error-Codes-Improvement-Proposal#json-rpc-standard-errors) would be returned `200` with error message.

| Code | Possible Return message | Description |
| :--- | :--- | :--- |
| -32700 | Parse error | Invalid JSON was received by the server. An error occurred on the server while parsing the JSON text. |
| -32600 | Invalid Request | The JSON sent is not a valid Request object. |
| -32601 | Method not found | The method does not exist / is not available. |
| -32602 | Invalid params | Invalid method parameter\(s\). |
| -32603 | Internal error | Internal JSON-RPC error. |
| -32000 to -32099 | `Server error`. Reserved for implementation-defined server-errors. | ​ |

```javascript
// http status: 200
{
    "jsonrpc":"2.0",
    "error":{
        "code":-32700,
        "message": "Parse error",
        "data":null},
    "id":1
}
```

