# Token

**Type:** class

The `alaio.token` sample system contract defines the structures and actions that allow users to create, issue, and manage tokens for ALAIO based blockchains. It demonstrates one way to implement a smart contract which allows for creation and management of tokens. It is possible for one to create a similar contract which suits different needs. However, it is recommended that if one only needs a token with the below listed actions, that one uses the `alaio.token` contract instead of developing their own.

The `alaio.token` contract class also implements two useful public static methods: `get_supply` and `get_balance`. The first allows one to check the total supply of a specified token, created by an account and the second allows one to check the balance of a token for a specified account (the token creator account has to be specified as well).

The `alaio.token` contract manages the set of tokens, accounts and their corresponding balances, by using two internal multi-index structures: the `accounts` and `stats`. The `accounts` multi-index table holds, for each row, instances of account object and the account object holds information about the balance of one token. The accounts table is scoped to an alaio account, and it keeps the rows indexed based on the token's symbol. This means that when one queries the accounts multi-index table for an account name the result is all the tokens that account holds at the moment.

Similarly, the stats multi-index table, holds instances of `currency_stats` objects for each row, which contains information about current supply, maximum supply, and the creator account for a symbol token. The `stats` table is scoped to the token symbol. Therefore, when one queries the `stats` table for a token symbol the result is one single entry/row corresponding to the queried symbol token if it was previously created, or nothing, otherwise.

## create

**Type:** void

Allows `issuer` account to create a token in supply of `maximum_supply`. If validation is successful a new entry in statstable for token symbol scope gets created.

Parameter Name | Description
--- | ---
issuer | - the account that creates the token,
maximum_supply | - the maximum supply set for the token created.

## issue

**Type:** void

This action issues to `to` account a `quantity` of tokens.

Parameter Name | Description
--- | ---
to | - the account to issue tokens to, it must be the same as the issuer,
quntity | - the amount of tokens to be issued,
## retire

**Type:** void

The opposite for create action, if all validations succeed, it debits the statstable.supply amount.

Parameter Name | Description
--- | ---
quantity | - the quantity of tokens to retire,
memo | - the memo string to accompany the transaction.

## transfer

**Type:** void

Allows from account to transfer to to account the quantity tokens. One account is debited and the other is credited with quantity tokens.

Parameter Name | Description
--- | ---
from | - the account to transfer from,
to | - the account to be transferred to,
quantity | - the quantity of tokens to be transferred,
memo | - the memo string to accompany the transaction.

## open

**Type:** void

Allows ram_payer to create an account owner with zero balance for token symbol at the expense of ram_payer.

Parameter Name | Description
--- | ---
owner | - the account to be created,
symbol | - the token to be payed with by ram_payer,
ram_payer | - the account that supports the cost of this action. More information can be read here and here.

## close

**Type:** void

This action is the opposite for open, it closes the account owner for token symbol.

Parameter Name | Description
--- | ---
owner | - the owner account to execute the close action for,
symbol | - the symbol of the token to execute the close action for.