# Chain API (1.0.0)

## get_account

Returns an object containing various details about a specific account on the blockchain.

### **Request Body**

key | description
--- | ---
account_name | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name)

### **Responses**



``` json
{
    "account_name": "string",
    "head_block_num": 0,
    "head_block_time": "string",
    "last_code_update": "string",
    "created": "string",
    "refund_request": {
        "owner": "string",
        "request_time": "string",
        "net_amount": "string",
        "cpu_amount": "string"
    },
"ram_quota": "string",
"net_limit": {
    "max": "string",
    "available": "string",
    "used": "string"
},
"cpu_limit": {
    "max": "string",
    "available": "string",
    "used": "string"
},
"total_resources": {
    "owner": "string",
    "ram_bytes": "string",
    "net_weight": "string",
    "cpu_weight": "string"
},
"core_liquid_balance": "string",
"self_delegated_bandwidth": {
    "from": "string",
    "to": "string",
    "net_weight": "string",
    "cpu_weight": "string"
},
"net_weight": "string",
"cpu_weight": "string",
"ram_usage": "string",
"privileged": true,
"permissions": [
    {
        "parent": "string",
        "perm_name": "string",
        "required_auth": {

            "waits": [
                {
                    "wait_sec": 0,
                    "weight": 0
                }
            ],
            "keys": [
                {
                    "key": "string",
                    "weight": 0
                }
            ],
            "threshold": 0,
            "accounts": [
                {
                    "weight": 0,
                    "permission": {
                        "actor": "string",
                        "permission": "string"
                    }
                }
            ]
        }
    }
],
"voter_info": {
    "owner": "string",
    "proxy": "string",
    "producers": [
            "string"
        ],
        "staked": "string",
        "last_vote_weight": "string",
        "proxied_vote_weight": "string",
        "is_proxy": 0,
        "flags1": 0,
        "reserved2": 0,
        "reserved3": "string"
    }
}
```

---

## get_block

Returns an object containing various details about a specific block on the blockchain.

### **Request Body**

key | description
--- | ---
block_num_or_id | `string` <br> Provide a `block number` or a `block id`

### **Responses**

``` json
{
    "timestamp": "string",
    "producer": "string",
    "confirmed": 0,
    "previous": "string",
    "transaction_mroot": "string",
    "action_mroot": "string",
    "schedule_version": 0,
    "new_producers": {
        "version": 0,
        "producers": [
            {
                "producer_name": "string",
                "block_signing_key": "string"
            }
        ]
    },
    "header_extensions": [
        0
    ],
    "new_protocol_features": [
        {}
    ],
    "producer_signature": "string",
    "transactions": [
        {
            "status": "executed",
            "cpu_usage_us": 0,
            "net_usage_words": 0,
            "trx": "string"
        }
    ],
    "block_extensions": [
        0
    ],
    "id": "string",
    "block_num": 0,
    "ref_block_prefix": 0
}
```

---

## get_info

Returns an object containing various details about the blockchain.

### **Responses**

```json
{
    "server_version": "string",
    "chain_id": "string",
    "head_block_num": 0,
    "head_block_id": "string",
    "head_block_time": "string",
    "head_block_producer": "string",
    "last_irreversible_block_num": 0,
    "last_irreversible_block_id": "string",
    "virtual_block_cpu_limit": 0,
    "virtual_block_net_limit": 0,
    "block_cpu_limit": 0,
    "block_net_limit": 0,
    "server_version_string": "string",
    "fork_db_head_block_num": 0,
    "fork_db_head_block_id": "string"
}
```

---

## push_transaction

This method expects a transaction in JSON format and will attempt to apply it to the blockchain.

### **Request body**

key | description
--- | ---
signatures | `Array of strings (Signature)` <br> array of signatures required to authorize transaction
compression | `boolean` <br> Compression used, usually false
packed_context_free_data | `string` <br> json to hex
packed_trx | `string` <br>  Transaction object json to hex

### **Responses**

    Returns nothing

---

## send_transaction

This method expects a transaction in JSON format and will attempt to apply it to the blockchain.

### **Request body**

key | description
--- | ---
signatures | `Array of strings (Signature)` <br> array of signatures required to authorize transaction
compression | `boolean` <br> Compression used, usually false
packed_context_free_data | `string` <br> json to hex
packed_trx | `string` <br> Transaction object json to hex

### **Responses**

    Returns nothing

---

## push_transactions

This method expects a transaction in JSON format and will attempt to apply it to the blockchain.

### **Request body**

key | description
--- | ---
expiration | `string (DateTime) ` <br> Time that transaction must be confirmed by.
ref_block_num | `integer`
ref_block_prefix | `integer` <br> 32-bit portion of block ID
max_net_usage_words | `string or integer (WholeNumber)` <br> A whole number
max_cpu_usage_ms | `string or integer (WholeNumber)` <br> A whole number
delay_sec | `integer`
context_free_actions | `Array of objects (Action)`
actions | `Array of objects (Action)`
transaction_extensions | `Array of Array of integers or strings (Extension)`

### **Responses**

    Returns nothing

---

## get_block_header_state

Retrieves the glock header state

### **Request body**

key | description
--- | ---
block_num_or_id | `string` <br> Provide a block_number or a block_id

### **Responses**

``` json
{
    "id": "string",
    "block_num": 0,
    "header": {
        "timestamp": "string",
        "producer": "string",
        "confirmed": 0,
        "previous": "string",
        "transaction_mroot": "string",
        "action_mroot": "string",
        "schedule_version": 0,
        "new_producers": {
            "version": 0,
            "producers": [
                {
                    "producer_name": "string",
                    "block_signing_key": "string"
                }
            ]
        },
        "header_extensions": [
            0
        ],
        "new_protocol_features": [
            {}
        ],
        "producer_signature": "string",
        "transactions": [
            {
                "status": "executed",
                "cpu_usage_us": 0,
                "net_usage_words": 0,
                "trx": "string"
            }
        ],
        "block_extensions": [
            0
        ],
        "id": "string",
        "block_num": 0,
        "ref_block_prefix": 0
    },
    "dpos_proposed_irreversible_blocknum": "string",
    "dpos_irreversible_blocknum": "string",
    "bft_irreversible_blocknum": "string",
    "pending_schedule_lib_num": "string",
    "pending_schedule_hash": "string",
    "pending_schedule": {
        "version": 0,
        "producers": [
            {
                "producer_name": "string",
                "block_signing_key": "string"
            }
        ]
    },
    "active_schedule": {
        "version": 0,
        "producers": [
            {
                "producer_name": "string",
                "block_signing_key": "string"
            }
        ]
    },
    "blockroot_merkle": {
        "_active_nodes": [
            "string"
        ],
        "_node_count": "string"
    },
    "producer_to_last_produced": [
        [
            "string"
        ]
    ],
    "producer_to_last_implied_irb": [
        [
            "string"
        ]
    ],
    "block_signing_key": "string",
    "confirm_count": [
        "string"
    ],
    "confirmations": [
        null
    ]
}
```

---

## get_abi

Retrieves the ABI for a contract based on its account name

### **Request body**

key | description
--- | ---
account_name | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name) 

### **Responses**

```json
{
    "version": "string",
    "types": [
        {
            "new_type_name": "string",
            "type": "uint8"
        }
    ],
    "structs": [
        {
            "name": "string",
            "base": "string",
            "fields": [
                {
                    "name": "string",
                    "type": "uint8"
                }
            ]
        }
    ],
    "actions": [
        {
            "name": "string",
            "type": "string",
            "ricardian_contract": "string"
        }
    ],
    "tables": [
        {
            "name": "string",
            "index_type": "i64",
            "key_names": [
                "string"
            ],
            "key_types": [
                "uint64"
            ],
            "type": "string"
        }
    ],
    "abi_extensions": [
        [
            0
        ]
    ],
    "error_messages": [
        "string"
    ],
    "ricardian_clauses": [
        "string"
    ],
    "variants": [
        "string"
    ]
}
```

---

## get_currency_balance

Retrieves the current balance

### **Request body**

key | description
--- | ---
code | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name) 
account | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name)
symbol | `string (Symbol) ^([0-9]{1,32}.[0-9]{4} [A-Z]{1,7})$ ` <br> A string representation of an ALAIO symbol, composed of a float with a precision of 4, and a symbol composed of capital letters between 1-7 letters separated by a space, example `1.0000 ABC`.

### **Responses**

```json
[
    "string"
]
```

---

## get_currency_stats

Retrieves currency stats

### **Request body**

key | description
--- | ---
code | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name)
symbol | `string (Symbol) ^([0-9]{1,32}.[0-9]{4} [A-Z]{1,7})$ ` <br> A string representation of an ALAIO symbol, composed of a float with a precision of 4, and a symbol composed of capital letters between 1-7 letters separated by a space, example `1.0000 ABC`.

### **Responses**

    Returns an object with one member labeled as the symbol you requested, the object has three members: supply (Symbol), max_supply (Symbol) and issuer (Name)

---

## get_required_keys

Returns the required keys needed to sign a transaction.

### **Request body**

key | description
--- | ---
transaction | object (Transaction) 
available_keys | `array of strings (PublicKey)` <br> Provide the available keys

### **Responses**

    null

---

## get_producers

Retrieves producers list

### **Request body**

key | description
--- | ---
limit | `string` <br> total number of producers to retrieve
lower_bound | `string` <br> In conjunction with limit can be used to paginate through the results. For example, limit=10 and lower_bound=10 would be page 2
json | `boolean` <br> return result in JSON format

### **Responses**

```json
{
    "active": [
        {
            "version": 0,
            "producers": [
                {
                    "producer_name": "string",
                    "block_signing_key": "string"
                }
            ]
        }
    ],
    "pending": [
        {
            "version": 0,
            "producers": [
                {
                    "producer_name": "string",
                    "block_signing_key": "string"
                }
            ]
        }
    ],
    "proposed": [
        {
            "version": 0,
            "producers": [
                {
                    "producer_name": "string",
                    "block_signing_key": "string"
                }
            ]
        }
    ]
}
```

---

## get_raw_code_and_abi

Retrieves raw code and ABI for a contract based on account name

### **Request body**

key | description
--- | ---
account_name | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name) 

### **Responses**

```json
{
    "account_name": "string",
    "wasm": "string",
    "abi": "string"
}
```

---

## get_scheduled_transaction

Retrieves the scheduled transaction

### **Request body**

key | description
--- | ---
lower_bound | `string (DateTimeSeconds) ` <br> Date/time string in the format YYYY-MM-DDTHH:MM:SS.sss
limit | `integer` <br> The maximum number of transactions to return
json | `boolean` <br> true/false whether the packed transaction is converted to json

### **Responses**

```json
{
    "transactions": [
        {
            "expiration": "string",
            "ref_block_num": 0,
            "ref_block_prefix": 0,
            "max_net_usage_words": "string",
            "max_cpu_usage_ms": "string",
            "delay_sec": 0,
            "context_free_actions": [
                {
                    "account": "string",
                    "name": "string",
                    "authorization": [
                        {
                            "actor": "string",
                            "permission": "string"
                        }
                    ],
                    "data": {},
                    "hex_data": "string"
                }
            ],
            "actions": [
                {
                    "account": "string",
                    "name": "string",
                    "authorization": [
                        {
                            "actor": "string",
                            "permission": "string"
                        }
                    ],
                    "data": {},
                    "hex_data": "string"
                }
            ],
            "transaction_extensions": [
                [
                    0
                ]
            ]
        }
    ]
}
```

---

## get_table_by_scope

Retrieves table scope

### **Request body**

key | description
--- | ---
code | `string` <br> `name` of the contract to return table data for
table | `string` <br> Filter results by table
lower_bound | `string` <br> Filters results to return the first element that is not less than provided value in set
upper_bound | `string` <br> Filters results to return the first element that is greater than provided value in set
limit | `integer <int32>` <br> Limit number of results returned.
reverse | `boolean` <br> Reverse the order of returned results

### **Responses**

```json
{
    "rows": [
        {
            "code": "string",
            "scope": "string",
            "table": "string",
            "payer": "string",
            "count": 0
        }
    ],
    "more": "string"
}
```

---

## get_table_rows

Returns an object containing rows from the specified table.

### **Request body**

key | description
--- | ---
code | `string` <br> The name of the smart contract that controls the provided table
table | `string` <br> The name of the table to query
scope | `string` <br> The account to which this data belongs
index_position | `string` <br> Position of the index used, accepted parameters `primary`, `secondary`, `tertiary`, `fourth`, `fifth`, `sixth`, `seventh`, `eighth`, `ninth` , `tenth`
key_type | `string` <br> Type of key specified by index_position (for example - `uint64_t` or `name`)
encode_type | `string`
upper_bound | `string`
lower_bound | `string`

### **Responses**

```json
{
    "rows": [
        null
    ]
}
```

---

## abi_json_to_bin

Returns an object containing rows from the specified table.

### **Request body**

key | description
--- | ---
binargs | string (Hex) ^(0[xX])?[0-9a-fA-F]*$ 

### **Responses**

``` json
{
    "binargs": "string"
}
```

---

## abi_bin_to_json

Returns an object containing rows from the specified table.

### **Request body**

key | description
--- | ---
code | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name) 
action | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name) 
binargs | string (Hex) ^(0[xX])?[0-9a-fA-F]*$ 

### **Responses**

    "string"

---

## get_code

Returns an object containing rows from the specified table.

### **Request body**

key | description
--- | ---
account_name | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name)
code_as_wasm | `integer` <br> default: 1 (true)

### **Responses**

```json
{
    "name": "string",
    "code_hash": "string",
    "wast": "string",
    "wasm": "string",
    "abi": {
        "version": "string",
        "types": [
            {
                "new_type_name": "string",
                "type": "uint8"
            }
        ],
        "structs": [
            {
                "name": "string",
                "base": "string",
                "fields": [
                    {
                        "name": "string",
                        "type": "uint8"
                    }
                ]
            }
        ],
        "actions": [
            {
                "name": "string",
                "type": "string",
                "ricardian_contract": "string"
            }
        ],
        "tables": [
            {
                "name": "string",
                "index_type": "i64",
                "key_names": [
                    "string"
                ],
                "key_types": [
                    "uint64"
                ],
                "type": "string"
            }
        ],
        "abi_extensions": [
            [
                0
            ]
        ],
        "error_messages": [
            "string"
        ],
        "ricardian_clauses": [
            "string"
        ],
        "variants": [
            "string"
        ]
    }
}
```

---

## get_raw_abi

Returns an object containing rows from the specified table.

### **Request body**

key | description
--- | ---
account_name | NamePrivileged (string) or NameBasic (string) or NameBid (string) or NameCatchAll (string) (Name)

### **Responses**

```json
{
    "account_name": "string",
    "code_hash": "string",
    "abi_hash": "string",
    "abi": "string"
}
```

---

## get_activated_protocol_features

Retreives the activated protocol features for producer node

### **Request body**

key | description
--- | ---
params | `object` <br> Defines the filters to retreive the protocol features by

### **Responses**

```json
{
    "activated_protocol_features": [
        "string"
    ],
    "more": 0
}
```

---

## get_accounts_by_authorizers

Given a set of account names and public keys, find all account permission authorities that are, in part or whole, satisfiable

### **Request body**

key | description
--- | ---
accounts | `Authority (object)` <br> List of authorizing accounts and/or actor/permissions
keys | `Array of strings (PublicKey) ` <br> List of authorizing keys

### **Responses**

```json
{
    "accounts": [
        {
            "account_name": "string",
            "permission_name": "string",
            "authorizer": "string",
            "weight": 0,
            "threshold": 0
        }
    ]
}
```