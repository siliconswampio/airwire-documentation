# Wallet Api Plugin

## Description

The `wallet_api_plugin` exposes functionality from the `wallet_plugin` to the RPC API interface managed by the `http_plugin`.

> **Caution** <br> <br> This plugin exposes wallets. Therefore, running this plugin on a publicly accessible node is not recommended. As of 1.2.0, the `wallet_api_plugin` is only available through `keosd`. It is no longer supported by `nodeos`.

## Usage

```
# config.ini
plugin = eosio::wallet_api_plugin

# command-line
`keosd` ... --plugin eosio::wallet_api_plugin
```

## Options

None

## Dependencies

* wallet_plugin
* http_plugin

### **Load Dependency Examples**

The following plugins are loaded with default settings if not specified on the command line or `config.ini`:

```
# config.ini
plugin = eosio::wallet_plugin
[options]
plugin = eosio::http_plugin
[options]

# command-line
keosd ... --plugin eosio::wallet_plugin [options]  \
           --plugin eosio::http_plugin [options]
```
