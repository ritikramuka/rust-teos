**THIS IS CURRENTLY WIP**

# The Eye of Satoshi (rust-teos)

The Eye of Satoshi is a Lightning watchtower compliant with [BOLT13](https://github.com/sr-gi/bolt13), written in Rust.

`rust-teos` consists of two main crates:

- `teos`: including the tower's main functionality (server-side) and a CLI.
- `teos-common`: including shared functionality between server and client-side (useful to build a client).

## Dependencies

Refer to [DEPENDENCIES.md](DEPENDENCIES.md)

## Installation
Refer to [INSTALL.md](INSTALL.md)

## Running TEOS

Make sure bitcoind is running before running `rust-teos` (it will fail at startup if it cannot connect to bitcoind). You can find
[here](DEPENDENCIES.md#installing-bitcoind) a sample config file.

### Starting the tower daemon ♖

Once installed, you can start the tower by running:

```
teosd
```

### Configuration file and command line parameters

`rust-teos` comes with a default configuration that can be found at [teos/src/config.rs](teos/src/config.rs). 

The configuration includes, amongst others, where your data folder is placed, what network it connects to, etc.

To change the configuration defaults you can:

- Define a configuration file named `teos.toml` following the template (check [teos/src/conf_template.toml](teos/src/conf_template.toml)) and place it in the `data_dir` (that defaults to `~/.teos/`)

and/or 

- Add some global options when running the daemon (run `teosd -h` for more info).

### Passing command-line options to `teosd`

Some configuration options can also be passed as options when running `teosd`. We can, for instance, change the tower data directory as follows:

```
teosd --datadir=<path_to_dir>
```

### Running TEOS in another network

By default, `teosd` runs on `mainnet`. In order to run it on another network, you need to change the network parameter in the configuration file or pass the network parameter as a command-line option. Notice that if teosd does not find a `bitcoind` node running in the same network that it is set to run, it will refuse to run hence change the network where teosd will run.

The configuration file option to change the network where `teos` will run is `btc_network`:

```
btc_network = mainnet
```

For regtest, it should look like:

```
btc_network = regtest
```

### Tower id and signing key

`teosd` needs a pair of keys that will serve as tower id and signing key. The former can be used by users to identify the tower, whereas the latter is used by the tower to sign responses. These keys are automatically generated on the first run and can be refreshed by running `teosd` with the `--overwritekey` flag. Notice that once a key is overwritten you won't be able to use the previous key again*.

\* Old keys are actually kept in the tower's database as a fail-safe in case you overwrite them by mistake. However, there is no automated way of switching back to an old key. Feel free to open an issue if you overwrote your key by mistake and need support to recover it.

## Interacting with a TEOS Instance

You can interact with a `teosd` instance (either run by yourself or someone else) by using `teos-cli`. This is an admin tool that has privileged access to the watchtower, and it should therefore only be used within a trusted environment (for example, the same machine).

While `teos-cli` works independently of `teosd`, it shares the same configuration file by default, of which it only uses a subset of its settings. The folder can be changed using the `--datadir` command-line argument if desired.

For help on the available arguments and commands, you can run:

```
teos-cli -h
```

## Interacting with TEOS as a client

FIXME: Add client and docs

## Contributing 
Refer to [CONTRIBUTING.md](CONTRIBUTING.md)
