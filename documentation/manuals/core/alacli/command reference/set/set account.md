# Set Account
## Description

set parameters dealing with account permissions

## Positionals

* `account` TEXT - The account to set/delete a permission authority for
* `permission` TEXT - The permission name to set/delete an authority for
* `authority` TEXT - [delete] NULL, [create/update] public key, JSON string, or filename defining the authority
* `parent` TEXT - [create] The permission name of this parents permission (Defaults to: "active")

## Options

* `-h,--help` Print this help message and exit
* `--add-code` [code] add 'alaio.code' permission to specified permission authority
* `--remove-code` [code] remove 'alaio.code' permission from specified permission authority
* `-x,--expiration` TEXT - set the time in seconds before a transaction expires, defaults to 30s
* `-f,--force-unique` - force the transaction to be unique. this will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times
* `-s,--skip-sign` Specify if unlocked wallet keys should be used to sign transaction
* `-d,--dont-broadcast` - Don't broadcast transaction to the network (just print to stdout)
* `-r,--ref-block` TEXT set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake)
* `-p,--permission` TEXT - An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active')
* `--max-cpu-usage-ms` UINT - set an upper limit on the milliseconds of cpu usage budget, for the execution of the transaction (defaults to 0 which means no limit)
* `--max-net-usage` UINT - set an upper limit on the net usage budget, in bytes, for the transaction (defaults to 0 which means no limit)
* `--delay-sec` UINT - set the delay_sec seconds, defaults to 0s

## Command

To modify the permissions of an account, you must have the authority over the account and the permission of which you are modifying. The set account permission command is subject to change so it's associated Class is not fully documented.

The first example associates a new key to the active permissions of an account.

    alacli set account permission test active '{"threshold":1,"keys":[{"key":"ALA8X7Mp7apQWtL6T2sfSZzBcQNUqZB7tARFEm9gA9Tn9nbMdsvBB","weight":1}],"accounts":[{"permission":{"actor":"acc2","permission":"active"},"weight":50}]}' owner

This second example modifies the same account permission, but removes the key set in the last example, and grants active authority of the test account to another account.

    alacli set account permission test active '{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"acc1","permission":"active"},"weight":50},{"permission":{"actor":"sandwich","permission":"active"},"weight":1}]}' owner

The third example demonstrates how to set up permissions for multisig.

    alacli set account permission test active '{"threshold" : 100, "keys" : [{"key":"ALA8X7Mp7apQWtL6T2sfSZzBcQNUqZB7tARFEm9gA9Tn9nbMdsvBB","weight":25}], "accounts" : [{"permission":{"actor":"sandwich","permission":"active"},"weight":75}]}' owner

The JSON object used in this command is actually composed of two different types of objects

The authority JSON object ...

    {
        "threshold"       : 100,    /*An integer that defines cumulative signature weight required for authorization*/
        "keys"            : [],     /*An array made up of individual permissions defined with an ALA PUBLIC KEY*/
        "accounts"        : []      /*An array made up of individual permissions defined with an ALA ACCOUNT*/
    }

...which includes one or more permissions objects.

    /*Set Permission with Key*/
    {
        "permission" : {
            "key"           : "ALA8X7Mp7apQWtL6T2sfSZzBcQNUqZB7tARFEm9gA9Tn9nbMdsvBB",
            "permission"    : "active"
        },
        weight            : 25      /*Set the weight of a signature from this permission*/
    }

    /*Set Permission with Account*/
    {
        "permission" : {
            "account"       : "sandwich",
            "permission"    : "active"
        },
        weight            : 75      /*Set the weight of a signature from this permission*/
    }