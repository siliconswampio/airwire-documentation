Currency Stats
Description

Retrieve the stats of for a given currency
Positional Parameters

contract TEXT - The contract that operates the currency

symbol TEXT - The symbol for the currency if the contract operates multiple currencies
Options

There are no options for this subcommand
Example

Get stats of the SYS token from the eosio.token contract.

cleos get currency stats eosio.token SYS

{
  "SYS": {
    "supply": "1000000000.0000 SYS",
    "max_supply": "10000000000.0000 SYS",
    "issuer": "eosio"
  }
}