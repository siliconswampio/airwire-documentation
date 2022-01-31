---
# Balance Snapshot Example
---

This example collects a snapshot of system token balances. At a requested block number it collects these for all accounts and saves the result
to a .csv file:
* Liquid system token balances
* Staked balances
* Pending stake refunds

It automatically determines the system token from the system contract's `rammarket` table. This example makes the following assumptions:
* The example `alaio.system` contract in https://github.com/ALADINIO/alaio.contracts is installed and initialized on the `alaio` account.
* The example `alaio.token` contract in https://github.com/ALADINIO/alaio.contracts is installed on the `alaio.token` account and manages the system token.

Note: use the `update_alaio_token_to_cdt1.6` branch (temporary) of `alaio.contracts` during build.

## balance.snap.hpp

```cpp
#pragma once
#include <alaio/asset.hpp>
#include <alaio/block_select.hpp>

struct base_request {
    uint32_t    block         = {};
    alaio::name first_account = {};
};

STRUCT_REFLECT(base_request) {
    STRUCT_MEMBER(base_request, block)
    STRUCT_MEMBER(base_request, first_account)
}

struct get_liquid_request : base_request {};
struct get_staked_request : base_request {};
struct get_refund_request : base_request {};

struct balance {
    alaio::name account = {};
    int64_t     amount  = {};
};

STRUCT_REFLECT(balance) {
    STRUCT_MEMBER(balance, account)
    STRUCT_MEMBER(balance, amount)
}

struct base_response {
    std::vector<balance>       balances = {};
    std::optional<alaio::name> more     = {};

    ALALIB_SERIALIZE(base_response, (balances)(more))
};

STRUCT_REFLECT(base_response) {
    STRUCT_MEMBER(base_response, balances)
    STRUCT_MEMBER(base_response, more)
}

struct get_liquid_response : base_response {};
struct get_staked_response : base_response {};
struct get_refund_response : base_response {};

using balance_snap_query_request = alaio::tagged_variant<
    alaio::serialize_tag_as_name,
    alaio::tagged_type<"get.liquid"_n, get_liquid_request>,
    alaio::tagged_type<"get.staked"_n, get_staked_request>,
    alaio::tagged_type<"get.refund"_n, get_refund_request>>;

using balance_snap_query_response = alaio::tagged_variant<
    alaio::serialize_tag_as_name,
    alaio::tagged_type<"get.liquid"_n, get_liquid_response>,
    alaio::tagged_type<"get.staked"_n, get_staked_response>,
    alaio::tagged_type<"get.refund"_n, get_refund_response>>;
```

## balance.snap-server.hpp

```cpp
#include "balance.snap.hpp"
#include <alaio/database.hpp>
#include <alaio/input_output.hpp>
#include <alaio/multi_index.hpp>
#include <alaio/types.h>

#include <alaio.system/alaio.system.hpp>

// refunds table in alaio.system
struct alaiosystem_refund_request {
    alaio::name           owner;
    alaio::time_point_sec request_time;
    alaio::asset          net_amount;
    alaio::asset          cpu_amount;
};

// Identify system token
alaio::symbol get_system_token(uint32_t block) {
    auto s = query_database(alaio::query_contract_row_range_code_table_scope_pk{
        .snapshot_block = block,
        .first =
            {
                .code        = "alaio"_n,
                .table       = "rammarket"_n,
                .scope       = "alaio"_n.value,
                .primary_key = 0,
            },
        .last =
            {
                .code        = "alaio"_n,
                .table       = "rammarket"_n,
                .scope       = "alaio"_n.value,
                .primary_key = 0xffff'ffff'ffff'ffff,
            },
        .max_results = 100,
    });

    alaio::symbol symbol;
    bool          found = false;
    alaio::for_each_contract_row<alaiosystem::exchange_state>(s, [&](alaio::contract_row& r, auto* state) {
        if (r.present && state) {
            alaio::check(!found, "Found more than 1 rammarket row");
            symbol = state->quote.balance.symbol;
            found  = true;
        }
        return true;
    });
    alaio::check(found, "Didn't find a rammarket row");
    return symbol;
}

// Fetch liquid balances
void process(get_liquid_request& req, const alaio::database_status& status) {
    if (req.block < status.first || req.block > status.irreversible)
        alaio::check(false, "requested block is out of range");
    auto system_token = get_system_token(req.block);

    auto s = query_database(alaio::query_contract_row_range_code_table_pk_scope{
        .snapshot_block = req.block,
        .first =
            {
                .code        = "alaio.token"_n,
                .table       = "accounts"_n,
                .primary_key = system_token.code().raw(),
                .scope       = req.first_account.value,
            },
        .last =
            {
                .code        = "alaio.token"_n,
                .table       = "accounts"_n,
                .primary_key = system_token.code().raw(),
                .scope       = 0xffff'ffff'ffff'ffff,
            },
        .max_results = 100,
    });

    get_liquid_response response;
    alaio::for_each_contract_row<alaio::asset>(s, [&](alaio::contract_row& r, alaio::asset* a) {
        if (r.present && a)
            response.balances.push_back(balance{alaio::name{r.scope}, a->amount});
        return true;
    });
    if (!response.balances.empty())
        response.more = alaio::name{response.balances.back().account.value + 1};
    alaio::set_output_data(pack(balance_snap_query_response{std::move(response)}));
}

// Fetch staked balances
void process(get_staked_request& req, const alaio::database_status& status) {
    if (req.block < status.first || req.block > status.irreversible)
        alaio::check(false, "requested block is out of range");
    auto system_token = get_system_token(req.block);

    auto s = query_database(alaio::query_contract_row_range_code_table_scope_pk{
        .snapshot_block = req.block,
        .first =
            {
                .code        = "alaio"_n,
                .table       = "voters"_n,
                .scope       = "alaio"_n.value,
                .primary_key = req.first_account.value,
            },
        .last =
            {
                .code        = "alaio"_n,
                .table       = "voters"_n,
                .scope       = "alaio"_n.value,
                .primary_key = 0xffff'ffff'ffff'ffff,
            },
        .max_results = 100,
    });

    get_staked_response response;
    alaio::for_each_contract_row<alaiosystem::voter_info>(s, [&](alaio::contract_row& r, alaiosystem::voter_info* info) {
        if (r.present && info)
            response.balances.push_back(balance{info->owner, info->staked});
        return true;
    });
    if (!response.balances.empty())
        response.more = alaio::name{response.balances.back().account.value + 1};
    alaio::set_output_data(pack(balance_snap_query_response{std::move(response)}));
}

// Fetch refund balances
void process(get_refund_request& req, const alaio::database_status& status) {
    if (req.block < status.first || req.block > status.irreversible)
        alaio::check(false, "requested block is out of range");
    auto system_token = get_system_token(req.block);

    auto s = query_database(alaio::query_contract_row_range_code_table_scope_pk{
        .snapshot_block = req.block,
        .first =
            {
                .code        = "alaio"_n,
                .table       = "refunds"_n,
                .scope       = req.first_account.value,
                .primary_key = 0,
            },
        .last =
            {
                .code        = "alaio"_n,
                .table       = "refunds"_n,
                .scope       = 0xffff'ffff'ffff'ffff,
                .primary_key = 0xffff'ffff'ffff'ffff,
            },
        .max_results = 100,
    });

    get_refund_response response;
    alaio::for_each_contract_row<alaiosystem_refund_request>(s, [&](alaio::contract_row& r, alaiosystem_refund_request* refund) {
        if (r.present && refund)
            response.balances.push_back(balance{refund->owner, refund->net_amount.amount + refund->cpu_amount.amount});
        return true;
    });
    if (!response.balances.empty())
        response.more = alaio::name{response.balances.back().account.value + 1};
    alaio::set_output_data(pack(balance_snap_query_response{std::move(response)}));
}

extern "C" __attribute__((alaio_wasm_entry)) void initialize() {}

extern "C" void run_query() {
    auto request = alaio::unpack<balance_snap_query_request>(alaio::get_input_data());
    std::visit([](auto& x) { process(x, alaio::get_database_status()); }, request.value);
}
```

## balance.snap-client.hpp

```cpp
#include "balance.snap.hpp"
#include <alaio/input_output.hpp>
#include <alaio/parse_json.hpp>
#include <alaio/schema.hpp>

extern "C" __attribute__((alaio_wasm_entry)) void initialize() {}
extern "C" void describe_query_request() { alaio::set_output_data(alaio::make_json_schema<balance_snap_query_request>()); }
extern "C" void describe_query_response() { alaio::set_output_data(alaio::make_json_schema<balance_snap_query_response>()); }

extern "C" void create_query_request() {
    alaio::set_output_data(
        pack(std::make_tuple("local"_n, "balance.snap"_n, alaio::parse_json<balance_snap_query_request>(alaio::get_input_data()))));
}

extern "C" void decode_query_response() {
    alaio::set_output_data(to_json(alaio::unpack<balance_snap_query_response>(alaio::get_input_data())));
}
```

## Building

Use the CDT to build the WASMs:

```sh
# Location of History Tools repository
export HT_TOOLS_DIR=~/history-tools

# Location of the CDT
export CDT_DIR=/usr/local/alaio.cdt

# Location of alaio.contracts
export CONTRACTS_DIR=~/alaio.contracts

$CDT_DIR/bin/alaio-cpp                                                      \
    -Os                                                                     \
    -I $CDT_DIR/include/alaiolib/capi                                       \
    -I $HT_TOOLS_DIR/libraries/alaiolib/wasmql                              \
    -I $HT_TOOLS_DIR/external/abiala/external/date/include                  \
    -I $CONTRACTS_DIR/contracts/alaio.system/include                        \
    $HT_TOOLS_DIR/libraries/alaiolib/wasmql/alaio/temp_placeholders.cpp     \
    -fquery-server                                                          \
    --alaio-imports=$HT_TOOLS_DIR/libraries/alaiolib/wasmql/server.imports  \
    balance.snap-server.cpp                                                 \
    -o balance.snap-server.wasm

$CDT_DIR/bin/alaio-cpp                                                      \
    -Os                                                                     \
    -I $HT_TOOLS_DIR/libraries/alaiolib/wasmql                              \
    -I $HT_TOOLS_DIR/external/abiala/external/date/include                  \
    -I $CONTRACTS_DIR/contracts/alaio.system/include                        \
    $HT_TOOLS_DIR/libraries/alaiolib/wasmql/alaio/temp_placeholders.cpp     \
    -fquery-client                                                          \
    --alaio-imports=$HT_TOOLS_DIR/libraries/alaiolib/wasmql/client.imports  \
    balance.snap-client.cpp                                                 \
    -o balance.snap-client.wasm
```

## Deploying

Place `balance.snap-server.wasm` in the directory that wasm-ql's `--wql-wasm-dir` option points to.

## nodejs Client

```js
const HistoryTools = require('./src/HistoryTools.js');
const fs = require('fs');
const { TextDecoder, TextEncoder } = require('util');
const fetch = require('node-fetch');
const encoder = new TextEncoder('utf8');
const decoder = new TextDecoder('utf8');

// Run one of the balance.snap queries and merge the results
// into accounts.
async function query(block, wasm, type, accounts, field) {
    let first_account = '';
    let num_found = 0;
    while (first_account != null) {
        // Create request
        const request = wasm.createQueryRequest(JSON.stringify(
            [type, {
                block,
                first_account,
            }]
        ));

        // Fetch reply
        const queryReply = await fetch('http://127.0.0.1:8880/wasmql/v1/query', {
            method: 'POST',
            body: HistoryTools.combineRequests([
                request,
            ]),
        });
        if (queryReply.status !== 200)
            throw new Error(queryReply.status + ': ' + queryReply.statusText + ': ' + await queryReply.text());

        // Convert from binary
        const responses = HistoryTools.splitResponses(new Uint8Array(await queryReply.arrayBuffer()));
        const response = JSON.parse(wasm.decodeQueryResponse(responses[0]))[1];

        // Merge result into accounts
        for (let bal of response.balances) {
            let account = accounts[bal.account] = accounts[bal.account] || {};
            account[field] = bal.amount;
        }
        num_found += response.balances.length;
        console.log(field, num_found, 'more=' + response.more);

        // Results span multiple queries
        first_account = response.more;
    }
}

if (process.argv.length != 3) {
    console.log('usage: nodejs balance.snap.js block_num');
} else {
    const block = +process.argv[2];
    (async () => {
        try {
            const myClientWasm = await HistoryTools.createClientWasm({
                mod: new WebAssembly.Module(fs.readFileSync('balance.snap-client.wasm')),
                encoder, decoder
            });
            let accounts = {};
            await query(block, myClientWasm, 'get.liquid', accounts, 'liquid');
            await query(block, myClientWasm, 'get.staked', accounts, 'staked');
            await query(block, myClientWasm, 'get.refund', accounts, 'refund');
            let csv = '';
            for (let name of Object.keys(accounts).sort()) {
                const acc = accounts[name];
                csv += `"${name}",${acc.liquid || 0},${acc.staked || 0},${acc.refund || 0}` + '\n';
            }
            fs.writeFileSync(block + '.csv', csv);
            console.log('wrote to ' + block + '.csv');
        } catch (e) {
            console.log(e);
        }
    })()
}
```
