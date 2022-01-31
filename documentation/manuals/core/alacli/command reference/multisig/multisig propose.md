# Multisig Propose Trx
## Description 

Propose transaction

## Positional Arguments

* proposal_name TEXT - Proposal name (string) 
* requested_permissions TEXT - The JSON string or filename defining requested permissions 
* trx_permissions TEXT - The JSON string or filename defining transaction permissions 
* contract TEXT - Contract to wich deferred transaction should be delivered 
* action TEXT - Action of deferred transaction 
* data TEXT - The JSON string or filename defining the action to propose 
* proposer TEXT - Account proposing the transaction 
* proposal_expiration UINT - Proposal expiration interval in hours

## Options

* -h,--help Print this help message and exit
* -x,--expiration TEXT - set the time in seconds before a transaction expires, defaults to 30s
* -f,--force-unique - force the transaction to be unique. this will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times
* -s,--skip-sign Specify if unlocked wallet keys should be used to sign transaction
* -d,--dont-broadcast - Don't broadcast transaction to the network (just print to stdout)
* -r,--ref-block TEXT set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake)
* -p,--permission TEXT - An account and permission level to authorize, as in 'account@permission' (defaults to 'account@active')
* --max-cpu-usage-ms UINT - set an upper limit on the milliseconds of cpu usage budget, for the execution of the transaction (defaults to 0 which means no limit)
* --max-net-usage UINT - set an upper limit on the net usage budget, in bytes, for the transaction (defaults to 0 which means no limit)
* --delay-sec UINT - set the delay_sec seconds, defaults to 0s
