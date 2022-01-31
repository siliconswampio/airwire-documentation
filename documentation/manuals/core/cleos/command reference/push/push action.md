# Push Action
## Description

Push a transaction with a single action

## Positionals

* `contract` Type: Text - The account providing the contract to execute
* `action` Type: Text - The action to execute on the contract
* `data` Type: Text - The arguments to the contract

**Output**

## Options

* `-h,--help` - Print this help message and exit
* `-x,--expiration` - set the time in seconds before a transaction expires, defaults to 30s
* `-f,--force-unique` - force the transaction to be unique. this will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times
* `-s,--skip-sign` - Specify if unlocked wallet keys should be used to sign transaction
* `-j,--json` - print result as json
* `-d,--dont-broadcast` - don't broadcast transaction to the network (just print to stdout)
* `-p,--permission` Type: Text - An account and permission level to authorize, as in 'account@permission'
* `--max-cpu-usage-ms` UINT - set an upper limit on the milliseconds of cpu usage budget, for the execution of the transaction (defaults to 0 which means no limit)
* `--max-net-usage` UINT - set an upper limit on the net usage budget, in bytes, for the transaction (defaults to 0 which means no limit)
* `--delay-sec` UINT - set the delay_sec seconds, defaults to 0s