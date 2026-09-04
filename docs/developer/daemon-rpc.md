# NERVA Daemon RPC
The daemon, `nervad`, exposes an HTTP interface that programs can use to talk to the node: query the chain, inspect the mempool, submit transactions, start and stop mining. Everything the CLI wallet, NervaOne and the block explorer do with a node goes through this interface. This page documents how to reach it, how to expose it safely, and what the main calls look like in practice.

The authoritative reference for every field of every call is the source itself, `src/rpc/core_rpc_server_commands_defs.h` in the [nerva repository][nerva-github-link]. This page stays at the level of what you need to get started and get work done.

# Ports and binding

On mainnet the daemon listens on three ports by default: 17565 for peer to peer traffic, 17566 for the HTTP RPC interface, and 17567 for a ZMQ RPC interface. The HTTP RPC binds to localhost only. A wallet on the same machine needs nothing more than a running daemon; remote access requires opting in.

The options that matter for the RPC are:

* `--rpc-bind-ip <ip>` — interface to bind, `127.0.0.1` by default
* `--rpc-bind-port <port>` — port to bind, 17566 by default on mainnet
* `--confirm-external-bind` — required confirmation when binding to something other than localhost
* `--rpc-login <user[:password]>` — enable HTTP digest authentication for the RPC
* `--rpc-access-control-origins <origins>` — allowed CORS origins, for browser applications
* `--restricted-rpc` — serve restricted mode on the main RPC port
* `--rpc-restricted-bind-port <port>` — serve restricted mode on a second port alongside the unrestricted one
* `--public-node` — run restricted RPC on the main port and advertise the node to peers as a public remote node

Restricted mode exists for pointing strangers at your node. It redacts information that could be used to fingerprint or attack the node: connection and peer counts are zeroed in `get_info`, peer details are hidden, and bulk output queries are capped. The `--public-node` flag is the friendly shortcut, since it also makes the node advertise itself so wallets like NervaOne can discover it.

Exposing an unrestricted, unauthenticated RPC to the internet is a bad idea, and the daemon will not let you do it by accident. If you need remote access, the usual combinations are restricted mode without authentication for read-only use, or an authenticated unrestricted port bound to the interfaces you actually need.

# Two ways to call

Most commands are reachable both as plain HTTP endpoints and as JSON-RPC 2.0 methods through the `/json_rpc` path. Both accept POST requests with a JSON body, and the choice is mostly a matter of taste. Simple queries read well as plain endpoints:

```
curl http://127.0.0.1:17566/get_info
```

The same information as a JSON-RPC call, which is how most existing integrations are written:

```
curl http://127.0.0.1:17566/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_info"}' -H 'Content-Type: application/json'
```

Either way the answer comes back as JSON with a `status` field that reads `OK` on success.

A few practical examples, all against a local mainnet daemon. The current height and top hash:

```
curl http://127.0.0.1:17566/get_height
```

The header of the latest block, with its hash, reward, timestamp and difficulty:

```
curl http://127.0.0.1:17566/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_last_block_header"}' -H 'Content-Type: application/json'
```

Where the network stands relative to the hard fork schedule, which is the call to watch around fork heights:

```
curl http://127.0.0.1:17566/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"hard_fork_info"}' -H 'Content-Type: application/json'
```

The total coins generated so far, a Nerva specific call that the website's supply counter uses:

```
curl http://127.0.0.1:17566/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_generated_coins"}' -H 'Content-Type: application/json'
```

The fee a transaction would currently pay, per kilobyte and for a given priority:

```
curl http://127.0.0.1:17566/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_fee_estimate","params":{"priority":1}}' -H 'Content-Type: application/json'
```

Transactions by hash, either your own or ones seen in the mempool, as full blobs:

```
curl http://127.0.0.1:17566/get_transactions -d '{"txs_hashes":["<txid>"]}' -H 'Content-Type: application/json'
```

Submitting an already signed transaction, which is what a wallet ultimately does:

```
curl http://127.0.0.1:17566/send_raw_transaction -d '{"tx_as_hex":"<hex>"}' -H 'Content-Type: application/json'
```

# What the interface offers

The plain HTTP endpoints cover status, chain data and node control. The frequently used ones:

| Endpoint | What it does |
|---|---|
| `/get_info` | height, difficulty, connections, version, fork state in one summary |
| `/get_height` | current height and top block hash |
| `/get_transaction_pool` | full mempool contents |
| `/get_transaction_pool_stats` | mempool counts, bytes, fees |
| `/get_transactions` | fetch transactions by hash |
| `/send_raw_transaction` | submit a signed transaction |
| `/get_peer_list` | white and grey peer lists |
| `/get_public_nodes` | nodes that announced themselves as public |
| `/mining_status` | whether the internal miner runs, and its settings |
| `/start_mining`, `/stop_mining` | control the internal miner |
| `/get_alt_blocks_hashes` | hashes of alternative blocks, if any |
| `/save_bc` | flush the blockchain database |
| `/pop_blocks` | roll the chain back, testing only |
| `/set_limit`, `/get_limit` | bandwidth limits |
| `/in_peers`, `/out_peers` | target peer counts |
| `/set_log_level`, `/set_log_categories` | logging |
| `/stop_daemon` | clean shutdown |
| `/update` | check for daemon updates |

The binary endpoints under `/get_blocks.bin`, `/get_hashes.bin`, `/get_outs.bin` and friends carry the bulk sync protocol and speak packed binary rather than JSON; they are what another daemon or a wallet uses to follow the chain, and rarely what an application calls by hand.

Through `/json_rpc`, the same server answers these methods:

| Method | What it does |
|---|---|
| `get_block_template` | block template for mining, with the correct reward |
| `submit_block` | submit a mined block |
| `get_last_block_header` | header of the tip |
| `get_block_header_by_hash`, `get_block_header_by_height` | header lookup |
| `get_block_headers_range` | a run of headers in one call |
| `get_block` | full block by hash or height |
| `on_get_block_hash` | height to hash, and yes, the name is really that |
| `get_info` | the same summary as the plain endpoint |
| `hard_fork_info` | current fork version, thresholds, heights |
| `get_min_version` | earliest daemon version still accepted on the network |
| `get_version` | RPC version of the daemon |
| `get_fee_estimate` | expected fee per kilobyte for a priority |
| `get_txpool_backlog` | mempool entries by fee and size |
| `get_output_histogram` | distribution of outputs by amount |
| `get_output_distribution` | amount distribution over heights, for decoy selection |
| `get_generated_coins` | total emitted coins so far |
| `get_tx_pubkey` | transaction public key for a txid |
| `decode_outputs` | decode outputs for given key images and view key |

Most of these have short legacy aliases (`getinfo`, `getlastblockheader` and so on) that older integrations still use; new code should prefer the full names.

# Public access without a node

For light integrations that only read chain data, the project operates a public HTTPS proxy in front of a daemon at `api.nerva.one`. Calls follow the pattern `https://api.nerva.one/daemon/<endpoint>/` with an empty JSON body:

```
curl -X POST https://api.nerva.one/daemon/get_info/ -d '{}'
```

It is the endpoint the website and several community tools use for live network figures. Treat it as a convenience for reading: keep anything involving wallets or private data on your own node.

Finally, the ZMQ interface on port 17567 publishes chain events such as new blocks and mempool updates to subscribers. It is enabled by default on localhost and suits monitoring dashboards and indexers that want push notifications instead of polling.

<!--Reference links -->
[nerva-github-link]: https://github.com/nerva-project/nerva
