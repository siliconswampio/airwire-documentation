# Currency Balance
## Description

Retrieve the balance of an account for a given currency

## Positional Parameters

* `contract` TEXT - The contract that operates the currency
* `account` TEXT - The account to query balances for
* `symbol` TEXT - The symbol for the currency if the contract operates multiple currencies

## Options

There are no options for this subcommand

## Example

Get balance of alaio from alaio.token contract for SYS symbol.

    alacli get currency balance alaio.token alaio SYS

```
999999920.0000 SYS
```