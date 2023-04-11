---
description: StarkEx Perpetual JSON-RPC Methods
---

# StarkEx

## starkex_getAccounts

Returns an array of public Stark keys, which correspond to key pairs available in the wallet for signing for the specific StarkEx application.

### Parameters

```
1. `system_id` : `String` - an id that uniquely identifies a StarkEx instance (per application, per deployment Mainnet/Testnet)
```

### Returns

```
1.`Array` - array of accounts:
    1.1. `Object` - object of account
        1.1.1. `pubkey` : `String` - public key for keypair
```

### Example

```javascript
// Request
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "starkex_getAccounts",
  "params": ["dydx_mainnet"]
}

// Result
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [{ "pubkey": "722RdWmHC5TGXBjTejzNjbc8xEiduVDLqZvoUGz6Xzbp" }]
}
```

## starkex_signTransaction

Signs an object that allows the wallet to easily parse the specific transaction type and to preview to the users what they are signing on.

### Parameters

```
1. `system_id` : `String` - an id that uniquely identifies a StarkEx instance (per application, per deployment Mainnet/Testnet)
2. `starkex_transaction`, `Object` : transaction to be signed
```

### Returns

```
1.`Object` - object of signature:
    1.1. `r` : `String` - the signature's r value
    1.2. `s` : `String` - the signature's s value
```
 

### Example

```javascript
// Request
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "starkex_signTransaction",
  "params": ["dydx_mainnet", {}]
}


// Result
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": { "r": "0x871082968778806658977547932973611445712745170287079421309719509169480683379", "s": "0x379123324788315712581779987541777480165529717324374846597948615743750904989" }
}
```

## starkex_signTransactionHash

Signs a hash, without previewing to the users what they are signing on (in this scenario, it is up for the application to create the hash correctly).

### Parameters

```
1. `system_id` : `String` - an id that uniquely identifies a StarkEx instance (per application, per deployment Mainnet/Testnet)
2. `transaction_hash`, `String` : transaction hash to be signed
```

### Returns

```
1.`Object` - object of signature:
    1.1. `r` : `String` - the signature's r value
    1.2. `s` : `String` - the signature's s value
```

### Example

```javascript
// Request
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "starkex_signTransactionHash",
  "params": ["dydx_mainnet", "0x48391715088fda539165055af5b4cde05367450899f4bdb74e832d8f940151e"]
}


// Result
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": { "r": "0x871082968778806658977547932973611445712745170287079421309719509169480683379", "s": "0x379123324788315712581779987541777480165529717324374846597948615743750904989" }
}
```
