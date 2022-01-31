# Pack Action Data

## Description

From json action data to packed form
## Positionals

* `account` TEXT - The name of the account that hosts the contract
* `name` TEXT - The name of the function that's called by this action
* `unpacked_action_data` TEXT - The action data expressed as json

## Options

* `-h,--help` - Print this help message and exit

## Usage

    alacli convert pack_action_data alaio unlinkauth '{"account":"test1", "code":"test2", "type":"alaioalaio"}'

## Output

    000000008090b1ca000000000091b1ca000075982aea3055