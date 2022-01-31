# Accounts
## Description

Retrieves all accounts associated with a defined public key

> This command will not return privileged accounts.

## Positional Parameters

* `public_key` TEXT - The public key to retrieve accounts for

## Options

* `-j,--json` - Output in JSON format

## Example

```
alacli get accounts ALA8mUftJXepGzdQ2TaCduNuSPAfXJHf22uex4u41ab1EVv9EAhWt
{
  "account_names": [
    "testaccount"
  ]
}
```