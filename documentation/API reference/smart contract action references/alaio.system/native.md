# Native

## permission

**Type:** struct

A weighted permission.

Defines a weighted permission, that is a permission which has a weight associated. A permission is defined by an account name plus a permission name.

## key

**Type:** struct

Weighted key.

A weighted key is defined by a public key and an associated weight.

## wait

**Type:** struct

Wait weight.

A wait weight is defined by a number of seconds to wait for and a weight.

## authority

**Type:** struct

Blockchain authority.

An authority is defined by:

* a vector of key_weights (a key_weight is a public key plus a wieght),
* a vector of permission_level_weights, (a permission_level is an account name plus a permission name)
* a vector of wait_weights (a wait_weight is defined by a number of seconds to wait and a weight)
* a threshold value

## block

**Type:** struct

Blockchain block header.

A block header is defined by:

* a timestamp,
* the producer that created it,
* a confirmed flag default as zero,
* a link to previous block,
* a link to the transaction merkel root,
* a link to action root,
* a schedule version,
* and a producers' schedule.

## abi

**Type:** struct

abi_hash is the structure underlying the abihash table and consists of:

* **owner:** the account owner of the contract's abi
* **hash:** is the sha256 hash of the abi/binary

## native

**Type:** class

The ALAIO core native contract that governs authorization and contracts' abi.

These actions map one-on-one with the ones defined in core layer of ALAIO, that's where their implementation actually is done. They are present here only so they can show up in the abi file and thus user can send them to this contract, but they have no specific implementation at this contract level, they will execute the implementation at the core layer and nothing else.

## newaccount

**Type:** void

New account action is called after a new account is created. This code enforces resource-limits rules for new accounts as well as new account naming conventions.

* accounts cannot contain '.' symbols which forces all acccounts to be 12 characters long without '.' until a future account auction process is implemented which prevents name squatting.
* new accounts must stake a minimal number of tokens (as set in system parameters) therefore, this method will execute an inline buyram from receiver for newacnt in an amount equal to the current new account creation fee.

## updateauth

**Type:** void

Update authorization action updates pemission for an account.

Parameter Name | Description
--- | ---
account | - the account for which the permission is updated
pemission | - the permission name which is updated
parem | - the parent of the permission which is updated
aut | - the json describing the permission authorization

## deleteauth

**Type:** void

Delete authorization action deletes the authorization for an account's permission.
Parameter Name | Description
--- | ---
account | - the account for which the permission authorization is deleted,
permission | - the permission name been deleted.

## linkauth

**Type:** void

Link authorization action assigns a specific action from a contract to a permission you have created. Five system actions can not be linked updateauth, deleteauth, linkauth, unlinkauth, and canceldelay. This is useful because when doing authorization checks, the ALAIO based blockchain starts with the action needed to be authorized (and the contract belonging to), and looks up which permission is needed to pass authorization validation. If a link is set, that permission is used for authoraization validation otherwise then active is the default, with the exception of alaio.any. alaio.any is an implicit permission which exists on every account; you can link actions to alaio.any and that will make it so linked actions are accessible to any permissions defined for the account.

Parameter Name | Description
--- | ---
account | - the permission's owner to be linked and the payer of the RAM needed to store this link,
code | - the owner of the action to be linked,
type | - the action to be linked,
requirement | - the permission to be linked.

## unlinkauth

**Type:** void

Unlink authorization action it's doing the reverse of linkauth action, by unlinking the given action.

Parameter Name | Description
--- | ---
account | - the owner of the permission to be unlinked and the receiver of the freed RAM,
code | - the owner of the action to be unlinked,
type | - the action to be unlinked.

## canceldelay

**Type:** void

Cancel delay action cancels a deferred transaction.

Parameter Name | Description
--- | ---
canceling_auth | - the permission that authorizes this action,
trx_id | - the deferred transaction id to be cancelled.

## onerror

**Type:** void

On error action, notification of this action is delivered to the sender of a deferred transaction when an objective error occurs while executing the deferred transaction. This action is not meant to be called directly.

Parameter Name | Description
--- | ---
sender_id | - the id for the deferred transaction chosen by the sender,
sent_trx | - the deferred transaction that failed.

## setabi

**Type:** void

Set abi action sets the contract abi for an account.

Parameter Name | Description
--- | ---
account | - the account for which to set the contract abi.
abi | - the abi content to be set, in the form of a blob binary.

## setcode

**Type:** void

Set code action sets the contract code for an account.

Parameter Name | Description
--- | ---
account | - the account for which to set the contract code.
vmtype | - reserved, set it to zero.
vmversion | - reserved, set it to zero.
code | - the code content to be set, in the form of a blob binary..