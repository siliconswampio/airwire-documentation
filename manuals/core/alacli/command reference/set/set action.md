# Set Action
## Description

Sets or updates an action's state on the blockchain.

**Command**

    alacli set action

**Output**

    Usage: alacli set action [OPTIONS] SUBCOMMAND

    Options:
    -h,--help                   Print this help message and exit

    Subcommands:
    permission                  set parmaters dealing with account permissions

**Command**

    alacli set action permission

## Positionals

* `account` TEXT The account to set/delete a permission authority for (required
* `code` Text The account that owns the code for the action
* `type` Type the type of the action
* `requirement` Type The permission name require for executing the given action

## Options

* `-h,--help` Print this help message and exit
* `-x,--expiration` Type:Text - set the time in seconds before a transaction expires, defaults to 30s
* `-f,--force-unique` - force the transaction to be unique. this will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times
* `-s,--skip-sign` Specify if unlocked wallet keys should be used to sign transaction
* `-j,--json` print result as json
* `-d,--dont-broadcast` - Don't broadcast transaction to the network (just print to stdout)
* `--return-packed` used in conjunction with --dont-broadcast to get the packed transaction
* `-r,--ref-block` TEXT set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake)
* `-p,--permission` Type:Text - An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active')
* `--max-cpu-usage-ms` UINT - Set an upper limit on the milliseconds of cpu usage budget, for the execution of the transaction (defaults to 0 which means no limit)
* `--max-net-usage` UINT - Set an upper limit on the net usage budget, in bytes, for the transaction (defaults to 0 which means no limit)
* `--delay-sec` UINT - set the delay_sec seconds, defaults to 0s

## Usage

    #Link a `voteproducer` action to the 'voting' permissions
    alacli set action permission sandwichfarm alaio.system voteproducer voting -p sandwichfarm@voting

    #Now can execute the transaction with the previously set permissions. 
    alacli system voteproducer approve sandwichfarm someproducer -p sandwichfarm@voting