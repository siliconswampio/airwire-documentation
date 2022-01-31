# Multisig

**Type:** class

The alaio.msig system contract allows for creation of proposed transactions which require authorization from a list of accounts, approval of the proposed transactions by those accounts required to approve it, and finally, it also allows the execution of the approved transactions on the blockchain.

In short, the workflow to propose, review, approve and then executed a transaction it can be described by the following:

* first you create a transaction json file,
* then you submit this proposal to the alaio.msig contract, and you also insert the account permissions required to approve this proposal into the command that submits the proposal to the blockchain,
* the proposal then gets stored on the blockchain by the alaio.msig contract, and is accessible for review and approval to those accounts required to approve it,
* after each of the appointed accounts required to approve the proposed transactions reviews and approves it, you can execute the proposed transaction. The alaio.msig contract will execute it automatically, but not before validating that the transaction has not expired, it is not cancelled, and it has been signed by all the permissions in the initial proposal's required permission list.

## propose

**Type:** void

Propose action, creates a proposal containing one transaction. Allows an account proposer to make a proposal proposal_name which has requested permission levels expected to approve the proposal, and if approved by all expected permission levels then trx transaction can we executed by this proposal. The proposer account is authorized and the trx transaction is verified if it was authorized by the provided keys and permissions, and if the proposal name doesn't already exist; if all validations pass the proposal_name and trx trasanction are saved in the proposals table and the requested permission levels to the approvals table (for the proposer context). Storage changes are billed to proposer.

Parameter Name | Description
--- | ---
proposer | - The account proposing a transaction
proposal_name | - The name of the proposal (should be unique for proposer)
requested | - Permission levels expected to approve the proposal
trx | - Proposed transaction

## approve

**Type:** void

Approve action approves an existing proposal. Allows an account, the owner of level permission, to approve a proposal proposal_name proposed by proposer. If the proposal's requested approval list contains the level permission then the level permission is moved from internal requested_approvals list to internal provided_approvals list of the proposal, thus persisting the approval for the proposal_name proposal. Storage changes are billed to proposer.

Parameter Name | Description
--- | ---
proposer | - The account proposing a transaction
proposal_name | - The name of the proposal (should be unique for proposer)
level | - Permission level approving the transaction
proposal_hash | - Transaction's checksum

## unapprove

**Type:** void

Unapprove action revokes an existing proposal. This action is the reverse of the approve action: if all validations pass the level permission is erased from internal provided_approvals and added to the internal requested_approvals list, and thus un-approve or revoke the proposal.

Parameter Name | Description
--- | ---
proposer | - The account proposing a transaction
proposal_name | - The name of the proposal (should be an existing proposal)
level | - Permission level revoking approval for proposal

## cancel

**Type:** void

Cancel action cancels an existing proposal.

Parameter Name | Description
--- | ---
proposer | - The account proposing a transaction
proposal_name | - The name of the proposal (should be an existing proposal)
canceler | - The account cancelling the proposal (only the proposer can cancel an unexpired transaction, and the canceler has to be different than the proposer) Allows the canceler account to cancel the proposal_name proposal, created by a proposer, only after time has expired on the proposed transaction. It removes corresponding entries from internal proptable and from approval (or old approvals) tables as well.

## exec

**Type:** void

Exec action allows an executer account to execute a proposal.

Preconditions:

* executer has authorization,
* proposal_name is found in the proposals table,
* all requested approvals are received,
* proposed transaction is not expired,
* and approval accounts are not found in invalidations table.

If all preconditions are met the transaction is executed as a deferred transaction, and the proposal is erased from the proposals table.

Parameter Name | Description
--- | ---
proposer | - The account proposing a transaction
proposal_name | - The name of the proposal (should be an existing proposal)
executer | - The account executing the transaction

## invalidate

**Type:** void

Invalidate action allows an account to invalidate itself, that is, its name is added to the invalidations table and this table will be cross referenced when exec is performed.

Parameter Name | Description
account | - The account invalidating the transaction