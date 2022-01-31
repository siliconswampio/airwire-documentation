# Wallet Plugin

## Description

The `wallet_plugin` adds access to wallet functionality from a node.

> Caution <br> <br> This plugin is not designed to be loaded as a plugin on a publicly accessible node without further security measures. This is particularly true when loading the `wallet_api_plugin`, which should not be loaded on a publicly accessible node under any circumstances.

## Usage

```
# config.ini
plugin = alaio::wallet_plugin

# command-line
alanode ... --plugin alaio::wallet_plugin
```

## Options

None

## Dependencies

* wallet_plugin
* http_plugin

### **Load Dependency Examples**

```
# config.ini
plugin = alaio::wallet_plugin
[options]
plugin = alaio::http_plugin
[options]

# command-line
alanode ... --plugin alaio::wallet_plugin [options]  \
           --plugin alaio::http_plugin [options]
```