---
# Creating WASMs
---

The sources for a minimal WASM pair include:
* A common header (`my.hpp`)
* A server implementation (`my-server.cpp`)
* A client implementation (`my-client.cpp`)

## Header

The header declares the set of available queries and responses. Here's an example header for a WASM pair which can query token balances for a range of accounts.

```cpp
// my.hpp

#pragma once
#include <alaio/asset.hpp>
#include <alaio/block_select.hpp>

// Parameters for a query
struct get_my_tokens_request {
    alaio::block_select snapshot_block = {};
    alaio::name         code           = {};
    alaio::symbol_code  sym            = {};
    alaio::name         first_account  = {};
    alaio::name         last_account   = {};
    uint32_t            max_results    = {};
};

// Enables JSON <> binary conversion
STRUCT_REFLECT(get_my_tokens_request) {
    STRUCT_MEMBER(get_my_tokens_request, snapshot_block)
    STRUCT_MEMBER(get_my_tokens_request, code)
    STRUCT_MEMBER(get_my_tokens_request, sym)
    STRUCT_MEMBER(get_my_tokens_request, first_account)
    STRUCT_MEMBER(get_my_tokens_request, last_account)
    STRUCT_MEMBER(get_my_tokens_request, max_results)
}

// A single balance
struct token_balance {
    alaio::name           account = {};
    alaio::extended_asset amount  = {};
};

STRUCT_REFLECT(token_balance) {
    STRUCT_MEMBER(token_balance, account)
    STRUCT_MEMBER(token_balance, amount)
}

// Query response
struct get_my_tokens_response {
    std::vector<token_balance> balances = {};
    std::optional<alaio::name> more     = {};

    ALALIB_SERIALIZE(get_my_tokens_response, (balances)(more))
};

STRUCT_REFLECT(get_my_tokens_response) {
    STRUCT_MEMBER(get_my_tokens_response, balances)
    STRUCT_MEMBER(get_my_tokens_response, more)
}

// The set of available queries. Each query has a name which identifies it.
using my_query_request = alaio::tagged_variant<
    alaio::serialize_tag_as_name,
    alaio::tagged_type<"get.my.toks"_n, get_my_tokens_request>>;

// The set of available responses. Each response has a name which identifies it.
// The name must match the request's name.
using my_query_response = alaio::tagged_variant<
    alaio::serialize_tag_as_name,
    alaio::tagged_type<"get.my.toks"_n, get_my_tokens_response>>;
```

## Server WASM

The server WASM uses the database to find answers to incoming requests.

```cpp
// my-server.cpp

#include "my.hpp"
#include <alaio/database.hpp>
#include <alaio/input_output.hpp>

// process this query
void process(get_my_tokens_request& req, const alaio::database_status& status) {

    // query the database for a range of contract rows,
    // ordered by (code, table, primary_key, scope)
    auto s = query_database(alaio::query_contract_row_range_code_table_pk_scope{
        // look at this point in time
        .snapshot_block = get_block_num(req.snapshot_block, status),

        // get records with keys >= first
        .first =
            {
                .code        = req.code,
                .table       = "accounts"_n,
                .primary_key = req.sym.raw(),
                .scope       = req.first_account.value,
            },

        // get records with keys <= last
        .last =
            {
                .code        = req.code,
                .table       = "accounts"_n,
                .primary_key = req.sym.raw(),
                .scope       = req.last_account.value,
            },

        // get this many results max. wasm-ql may return fewer results.
        .max_results = req.max_results,
    });

    get_my_tokens_response response;

    // loop through each record
    alaio::for_each_contract_row<alaio::asset>(s, [&](alaio::contract_row& r, alaio::asset* a) {
        // let requestor know how to continue the search
        response.more = alaio::name{r.scope + 1};

        // if the row is present and contains a valid asset, record the result
        if (r.present && a)
            response.balances.push_back({
                .account = alaio::name{r.scope},
                .amount = alaio::extended_asset{*a, req.code}
            });

        // continue the loop
        return true;
    });

    // send the result to the requestor
    alaio::set_output_data(pack(my_query_response{std::move(response)}));
}

// initialize this WASM
extern "C" __attribute__((alaio_wasm_entry)) void initialize() {}

// wasm-ql calls this for each incoming query
extern "C" void run_query() {
    // deserialize the request
    auto request = alaio::unpack<my_query_request>(alaio::get_input_data());

    // dispatch the request to the appropriate `process` overload
    std::visit([](auto& x) { process(x, alaio::get_database_status()); }, request.value);
}
```

## Client WASM

The client WASM converts between JSON and the binary format the server WASM understands

```cpp
// my-client.cpp

#include "my.hpp"
#include <alaio/input_output.hpp>
#include <alaio/parse_json.hpp>
#include <alaio/schema.hpp>

// initialize this WASM
extern "C" __attribute__((alaio_wasm_entry)) void initialize() {}

// produce JSON schema for request
extern "C" void describe_query_request() {
    alaio::set_output_data(alaio::make_json_schema<my_query_request>());
}

// produce JSON schema for response
extern "C" void describe_query_response() {
    alaio::set_output_data(alaio::make_json_schema<my_query_response>());
}

// convert request from JSON to binary
extern "C" void create_query_request() {
    alaio::set_output_data(pack(std::make_tuple(
        "local"_n,      // must be "local"
        "my"_n,         // name of server WASM
        alaio::parse_json<my_query_request>(alaio::get_input_data()))));
}

// convert response from binary to JSON
extern "C" void decode_query_response() {
    alaio::set_output_data(to_json(alaio::unpack<my_query_response>(alaio::get_input_data())));
}
```

## Building

Use the CDT to build the WASMs:

```sh
# Location of History Tools repository
export HT_TOOLS_DIR=~/history-tools

# Location of the CDT
export CDT_DIR=/usr/local/alaio.cdt

$CDT_DIR/bin/alaio-cpp                                                      \
    -Os                                                                     \
    -I $HT_TOOLS_DIR/libraries/alaiolib/wasmql                              \
    -I $HT_TOOLS_DIR/external/abiala/external/date/include                  \
    $HT_TOOLS_DIR/libraries/alaiolib/wasmql/alaio/temp_placeholders.cpp     \
    -fquery-server                                                          \
    --alaio-imports=$HT_TOOLS_DIR/libraries/alaiolib/wasmql/server.imports  \
    my-server.cpp                                                           \
    -o my-server.wasm

$CDT_DIR/bin/alaio-cpp                                                      \
    -Os                                                                     \
    -I $HT_TOOLS_DIR/libraries/alaiolib/wasmql                              \
    -I $HT_TOOLS_DIR/external/abiala/external/date/include                  \
    $HT_TOOLS_DIR/libraries/alaiolib/wasmql/alaio/temp_placeholders.cpp     \
    -fquery-client                                                          \
    --alaio-imports=$HT_TOOLS_DIR/libraries/alaiolib/wasmql/client.imports  \
    my-client.cpp                                                           \
    -o my-client.wasm
```

## Deploying

Place `my-server.wasm` in the directory that wasm-ql's `--wql-wasm-dir` option points to. Make `my-client.wasm` available to clients.

## Example use

Modify the code in [Using Client WASMs](using-client-wasms.md):

```js
// Use this for browsers:
const myClientWasm = await HistoryTools.createClientWasm({
    mod: await WebAssembly.compileStreaming(fetch('.../path/to/my-client.wasm')),
    encoder, decoder
});

// Use this for nodejs
const myClientWasm = await HistoryTools.createClientWasm({
    mod: new WebAssembly.Module(fs.readFileSync('.../path/to/my-client.wasm')),
    encoder, decoder
});

// Use this for either:
const request = myClientWasm.createQueryRequest(JSON.stringify(
    ['get.my.toks', {
        snapshot_block: ['head', 0],
        code: 'alaio.token',
        sym: 'ALA',
        first_account: 'alaio',
        last_account: 'alaio.zzzzzz',
        max_results: 10,
    }]
));

const queryReply = await fetch('http(s)://server.name:port/wasmql/v1/query', {
    method: 'POST',
    body: HistoryTools.combineRequests([
        request,
    ]),
});
if (queryReply.status !== 200)
    throw new Error(queryReply.status + ': ' + queryReply.statusText + ': ' + await queryReply.text());

const responses = HistoryTools.splitResponses(new Uint8Array(await queryReply.arrayBuffer()));

prettyPrint('The tokens', myClientWasm.decodeQueryResponse(responses[0]));
```
