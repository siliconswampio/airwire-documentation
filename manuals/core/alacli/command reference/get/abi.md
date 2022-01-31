# Abi
## Description

Retrieves the ABI for an account

## Positional Parameters

* `name` TEXT - The name of the account whose abi should be retrieved

## Options

* `-f,--file` TEXT - The name of the file to save the contract .abi to instead of writing to console

## Examples

Retrieve and save abi for alaio.token contract

```
alacli get abi alaio.token -f alaio.token.abi
```

```
saving abi to alaio.token.abi
```