# Rex

**Type:** class

The actions `buyresult`, `sellresult`, `rentresult`, and `orderresult` of `rex.results` are all no-ops. They are added as inline convenience actions to rentnet, rentcpu, buyrex, unstaketorex, and sellrex. An inline convenience action does not have any effect, however, its data includes the result of the parent action and appears in its trace.

## buyresult
[rex.result::buyresult]:#buyresult

**Type:** void

Buyresult action.

Parameter Name | Description
--- | ---
rex_received | - amount of tokens used in buy order

## sellresult
[rex.result::sellresult]:#sellresult

**Type:** void

Sellresult action.

Parameter Name | Description
--- | ---
proceeds | - amount of tokens used in sell order

## orderresult
[rex.result::orderresult]:#orderresult

**Type:** void

Orderresult action.

Parameter Name | Description
--- | ---
owner | - the owner of the order
proceeds | - amount of tokens used in order

## rentresult
[rex.result::rentresult]:#rentresult

**Type:** void

Rentresult action.

Parameter Name | Description
--- | ---
rented_tokens | - amount of rented tokens