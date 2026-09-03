# NERVA Wallet RPC
The command line wallet has a server twin, `nerva-wallet-rpc`, that exposes the same wallet engine over HTTP. It is what you build on when a program needs to hold Nerva: a payment processor watching for incoming transfers, a storefront signing withdrawals, NervaOne itself, which drives the wallet this way. This page explains how to run it, how its authentication works, and what the interface looks like in practice.

The interface is close kin to the daemon's: POST requests with JSON bodies, digest authentication, and a JSON-RPC flavor through `/json_rpc` plus a set of plain endpoints. The full field-by-field reference lives in `src/wallet/wallet_rpc_server_commands_defs.h` in the [nerva repository][nerva-github-link].

# Starting the server

The wallet server needs to know which wallet to serve, where to listen, and where the daemon is. The minimal single wallet invocation:

    nerva-wallet-rpc --wallet-file /path/to/wallet.wallet --rpc-bind-port 18082

There is no default port: `--rpc-bind-port` is required, and the server binds to localhost only. The wallet file, its password and its cache are handled exactly like in the CLI wallet, so a wallet that opens in `nerva-wallet-cli` opens here too. You will be prompted for the password on startup unless you pass `--password` or start with `--prompt-for-password`.

The other mode serves a directory of wallets instead of one:

    nerva-wallet-rpc --wallet-dir /path/to/wallets --rpc-bind-port 18082

In this mode no wallet is open at startup; clients create, open, close and switch wallets at runtime through the RPC itself, which suits servers handling several wallets under one roof. `--wallet-file` and `--wallet-dir` are mutually exclusive.

Connection to the daemon is configured with `--daemon-address <host:port>`, which defaults to a daemon on localhost, and `--trusted-daemon` or `--untrusted-daemon` to say whether the node is your own. Trust matters more than the name suggests: some wallet conveniences, like spending unconfirmed change or importing key images, are only safe against a node you control. The server also refuses daemons speaking an incompatible RPC major version, which is usually exactly what you want during hard forks; `--allow-mismatched-daemon-version` trades that safety for uptime.

# Authentication

The wallet server enables HTTP digest authentication by default, and this is one case where the defaults genuinely protect you: the RPC can spend money. If you start it without `--rpc-login <user:pass>`, it generates a random password itself, for user `nerva`, and writes the credentials to a login file in the working directory:

    RPC username/password is stored in file nerva-wallet-rpc.18082.login

Read that file, use those credentials in your client, and delete the file if it bothers you. Passing `--rpc-login myuser:mypassword` sets your own instead. `--disable-rpc-login` turns authentication off entirely, and should only ever be used on a loopback interface that nothing else can reach, because anyone who can talk to the server can spend from the wallet.

For browser clients, `--rpc-access-control-origins` sets the CORS policy, same as on the daemon.

# Calling it

A wallet directory server, freshly started, is an empty stage. Create a wallet:

```
curl http://127.0.0.1:18082/json_rpc -u nerva:<password> \
  -d '{"jsonrpc":"2.0","id":"0","method":"create_wallet","params":{"filename":"shop","password":"walletpass","language":"English"}}' \
  -H 'Content-Type: application/json'
```

Wallets are addressed by filename within the directory, and each call that touches a locked wallet takes its password in `params`. The newly created wallet is open and ready; on a server restarted later, `open_wallet` with the same filename and password brings it back.

The balance, split into total and unlocked, plus per-account and per-subaddress detail:

```
curl http://127.0.0.1:18082/json_rpc -u nerva:<password> \
  -d '{"jsonrpc":"2.0","id":"0","method":"get_balance","params":{"account_index":0}}' \
  -H 'Content-Type: application/json'
```

Incoming transfers are most easily watched with `get_transfers`, filtered by type, say `{"in":true,"pending":true,"pool":true}` for everything arriving, or `get_bulk_payments` keyed by payment ids or subaddresses when you are matching orders to payments.

Sending is one call. Note that amounts are in atomic units, where one XNV is one trillion units, 1 followed by 12 zeros:

```
curl http://127.0.0.1:18082/json_rpc -u nerva:<password> \
  -d '{"jsonrpc":"2.0","id":"0","method":"transfer","params":{"destinations":[{"amount":1000000000000,"address":"NV..."}],"priority":1}}' \
  -H 'Content-Type: application/json'
```

The response carries the fee actually paid and the transaction hash. For larger or privacy sensitive software there are staged variants: `transfer_split` builds one transaction per necessary ring, `describe_transfer`, `sign_transfer` and `submit_transfer` separate construction from signing for cold wallet setups, and `sweep_all` and `sweep_single` empty an address or output.

# What the interface offers

Grouped by what you are probably trying to do:

| Group | Methods |
|---|---|
| Wallet files | `create_wallet`, `open_wallet`, `close_wallet`, `change_wallet_password`, `restore_wallet_from_seed`, `restore_wallet_from_keys`, `store` |
| Addresses and accounts | `get_address`, `get_address_index`, `create_address`, `label_address`, `get_accounts`, `create_account`, `label_account`, account tagging |
| Balance and history | `get_balance`, `get_transfers`, `get_transfer_by_txid`, `get_payments`, `get_bulk_payments`, `incoming_transfers` |
| Spending | `transfer`, `transfer_split`, `describe_transfer`, `sign_transfer`, `submit_transfer`, `sweep_all`, `sweep_single`, `sweep_dust`, `sweep_unmixable`, `relay_tx` |
| Keys and proofs | `query_key`, `get_tx_key`, `check_tx_key`, `get_tx_proof`, `check_tx_proof`, `get_spend_proof`, `check_spend_proof`, `get_reserve_proof`, `check_reserve_proof` |
| Address book | `get_address_book`, `add_address_book`, `edit_address_book`, `delete_address_book` |
| URIs and integrated addresses | `make_uri`, `parse_uri`, `make_integrated_address`, `split_integrated_address` |
| Node and housekeeping | `refresh`, `auto_refresh`, `rescan_blockchain`, `rescan_spent`, `set_daemon`, `start_mining`, `stop_mining`, `set_donate_level`, `set_log_level`, `set_log_categories`, `get_height`, `get_version`, `get_languages`, `validate_address`, `set_attribute`, `get_attribute`, `stop_wallet` |
| Low level | `sign`, `verify`, `export_outputs`, `import_outputs`, `export_key_images`, `import_key_images` |

The multisig methods, from `is_multisig` through `sign_multisig`, are present in the interface but refuse to run: multisig is a disabled legacy feature in Nerva, and existing multisig wallets lose the ability to spend at hard fork 14. The details are on the [Multisig Guide][multisig-guide-link].

# Running it responsibly

The wallet server holds the keys to real money, so the boring precautions matter. Keep it on loopback and put a reverse proxy in front if a remote client needs access, which also gives you TLS; the RPC options include SSL settings for exactly that purpose. Give the daemon it talks to the same care, since an untrusted daemon cannot steal keys but can feed the wallet misleading chain data. And take backups seriously: the login file is a convenience, but the mnemonic seed you printed at creation time is the real backup, and no RPC call replaces it.

<!--Reference links -->
[nerva-github-link]: https://github.com/nerva-project/nerva
[multisig-guide-link]: ../guides/multisig.md
