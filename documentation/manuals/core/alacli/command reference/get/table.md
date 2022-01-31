# Table
## Description

Retrieves the contents of a database table

## Positional Parameters

`contract` TEXT - The contract who owns the table

`scope` TEXT - The scope within the contract in which the table is found

`table` TEXT - The name of the table as specified by the contract abi

## Options

`-b,--binary` UINT - Return the value as BINARY rather than using abi to interpret as JSON

`-l,--limit` UINT - The maximum number of rows to return

`-k,--key` TEXT - The name of the key to index by as defined by the abi, defaults to primary key

`-L,--lower` TEXT - JSON representation of lower bound value of key, defaults to first

`-U,--upper` TEXT - JSON representation of upper bound value value of key, defaults to last

## Example

Get the data from the accounts table for the alaio.token contract, for user alaio,

    alacli get table alaio.token alaio accounts

```
{
  "rows": [{
      "balance": "999999920.0000 SYS"
    }
  ],
  "more": false
}
```