---
content_title: wasm-ql
---

```dot-svg

#db filler, db, wasm-ql, web browser - db_wasmql_web_flow.dot
#
#notes: * to see image copy/paste to https://dreampuf.github.io/GraphvizOnline
#       * image will be rendered by gatsby-remark-graphviz plugin in eosio docs.
#
#+----------+    +------------+    +---------------+       +-------------------+
#| database |    | database   |    | wasm-ql       |       | web browser       |
#| filler   |    |            |    | ------------- |       | ---------------   |
#|          | => | PostgreSQL | => | Server WASM A |  <=>  | Client WASM A     |
#|          |    |     or     |    | Server WASM B |  <=>  | Client WASM B     |
#|          |    |   RocksDB  |    | ...           |       |                   |
#|          |    |            |    | Legacy WASM   |  <=>  | js using /v1/ RPC |
#+----------+    +------------+    +---------------+       +-------------------+

digraph {
    graph [rankdir=LR, splines=ortho]
    node [shape=box, height=1.2, fontname=arial, fontsize=11]
    edge [arrowsize=.75]

    db_filler [label="database\nfiller"]
    db [label="database\n\nPostgreSQL\nor\nRocksDB"]
    wasmql [label="wasm-ql\n\nServer WASM A\nServer WASM B\n...\nLegacy WASM"]
    web_browser [label="web browser\n\nClient WASM A\nClient WASM B\n \njs using /v1/ RPC"]

    db_filler -> db -> wasmql
    wasmql -> web_browser [color=invis]
    wasmql -> web_browser [dir=both]
    wasmql -> web_browser [color=invis]
    wasmql -> web_browser [dir=both]
    wasmql -> web_browser [dir=both]
    wasmql -> web_browser [color=invis]
}
```

wasm-ql listens on an http port and answers the following:
* POST binary data to `http://host:port/wasmql/v1/query`
  * The binary data includes a set of queries directed to particular server WASMs
  * wasm-ql passes each query to the appropriate WASM
  * wasm-ql collects the query replies and produces a binary response containing the replies
* POST JSON query to `http://host:port/v1/*`
  * The legacy server WASM handles these requests
  * The WASM produces a JSON response
  * wasm-ql forwards the response to the client

Client WASMs provide these functions to js clients:
* `create_query_request()`: Convert a JSON request to the binary format the server WASM expects
* `decode_query_response()`: Convert a binary response from the server WASM to JSON
* `describe_query_request()`: Describes the JSON request format to clients using JSON Schema
* `describe_query_response()`: Describes the JSON response format to clients using JSON Schema

These legacy API functions are available to clients:
* `/v1/chain/get_abi`: Retrieves the ABI of an account, if any.
* `/v1/chain/get_account`: Retrieves account information, including code and abi but not including
  quotas, weights, or permissions.
* `/v1/chain/get_block`: Retrieves block information. Does not include producer signature or transactions.
* `/v1/chain/get_code`: Retrieves the WASM of an account, if any.
* `/v1/chain/get_currency_balance`: Retrieves currency balance in the specified token for the given account
  accounted for with the given token account.
* `/v1/chain/get_producer_schedule`: Retrieves up to 21 producers sorted by most votes.
* `/v1/chain/get_table_rows`: Retrieves rows from arbitrary tables created by contracts.
* `/v1/history/get_transaction`: Retrieves a transaction by transaction id.
* `/v1/history/get_actions`: Retrieves transaction actions affecting the given receipt receiver.
