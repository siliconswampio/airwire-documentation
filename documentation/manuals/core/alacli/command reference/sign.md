# Sign
## Description

Sign a transaction
## Usage

    alacli sign [OPTIONS] transaction

Positional Parameters

* `transaction` TEXT - The JSON string or filename defining the transaction to sign

Options

* `-k,--private-key` TEXT - The private key that will be used to sign the transaction
* `--public-key` TEXT - Ask kalad to sign with the corresponding private key of the given public key
* `-c,--chain-id` TEXT - The chain id that will be used to sign the transaction
* `-p,--push-transaction` - Push transaction after signing
