# Code
## Description

Retrieves the code and ABI for an account
## Positional Parameters

* `name` TEXT - The name of the account whose code should be retrieved

## Options

* `-c,--code` TEXT - The name of the file to save the contract .wast to
* `-a,--abi` TEXT - The name of the file to save the contract .abi to
* `--wasm` Save contract as wasm

## Examples

Simply output the hash of eosio.token contract

    cleos get code eosio.token


```
code hash: f675e7aeffbf562c033acfaf33eadff255dacb90d002db51c7ad7cbf057eb791
```

Retrieve and save abi for eosio.token contract

    cleos get code eosio.token -a eosio.token.abi

```
code hash: f675e7aeffbf562c033acfaf33eadff255dacb90d002db51c7ad7cbf057eb791
saving abi to eosio.token.abi
```

Retrieve and save wast code for eosio.token contract

    cleos get code eosio.token -c eosio.token.wast

```
code hash: f675e7aeffbf562c033acfaf33eadff255dacb90d002db51c7ad7cbf057eb791
saving wast to eosio.token.wast
```