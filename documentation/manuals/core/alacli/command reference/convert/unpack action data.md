# Unpack Action Data
## Description

From packed to json action data form
Positionals

* `account` TEXT - The name of the account that hosts the contract
* `name` TEXT - The name of the function that's called by this action
* `packed_action_data` TEXT - The action data expressed as packed hex string

## Options

* `-h,--help` - Print this help message and exit

## Usage

    alacli convert unpack_action_data alaio unlinkauth 000000008090b1ca000000000091b1ca000075982aea3055

## Output
```json
{
  "account": "test1",
  "code": "test2",
  "type": "alaioalaio"
}
```