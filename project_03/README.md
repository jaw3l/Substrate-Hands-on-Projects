# Hands on Project - 03

## Introduction

In this project, we be controlling which nodes can join our network. Only authorized nodes will be able to join our network.

![INFO]
The changes made to the files can be found in the my fork of the `substrate-node-template` repository. You can reach the repository [here](https://github.com/jaw3l/substrate-node-template/tree/learn_substrate).

## Prerequisites

Prerequisites are the same as the previous project for the most part. You can find them [here](../project_01/README.md#prerequisites).

The only difference is we will need two accounts to simulate two nodes. You can find more information in [Linux Manual Pages](https://linux.die.net/man/8/adduser).

## Setup

### Generating Keys

First, we need to generate keys for our nodes. We will do that from our main account. We can run the interactive prompt with the following command:

```bash
./target/release/node-template key generate --scheme Sr25519 --password-interactive
```

We will be prompted to enter a password. After entering the password, we will get output similar to the following:

```log
Secret phrase:       apology stairs vacuum glad awake survey universe employ use fiscal focus assist
  Network ID:        substrate
  Secret seed:       0xb43bf35fac5890f340af6258214416c98280f37271ced0e921face2d9b6d0cc0
  Public key (hex):  0x2c95b595b683c075875f594159884ece7b75d36cdc43266d825a9dd0cccc2362
  Account ID:        0x2c95b595b683c075875f594159884ece7b75d36cdc43266d825a9dd0cccc2362
  Public key (SS58): 5D5ASJU6hUc2oJ8hmZ1xS28xEqxAtfbL1wFqkjvKCnKppZjV
  SS58 Address:      5D5ASJU6hUc2oJ8hmZ1xS28xEqxAtfbL1wFqkjvKCnKppZjV
```

What we have here is our Sr25519 key for producing blocks using `aura` for one node. We will need the `Secret phrase` for the next step.

To derive keys for the account we just created using our `Secret phrase`, we can run the following command:

```bash
./target/release/node-template key inspect --password-interactive --scheme Ed25519 "apology stairs vacuum glad awake survey universe employ use fiscal focus assist"
```

We will be prompted to enter the password. After entering the password, we will get output similar to the following:

```log
Secret phrase:       apology stairs vacuum glad awake survey universe employ use fiscal focus assist
  Network ID:        substrate
  Secret seed:       0xb43bf35fac5890f340af6258214416c98280f37271ced0e921face2d9b6d0cc0
  Public key (hex):  0x5404aee5dfc6aee1acfab0f18a7b3bf88738a1aae4778194ff627da7e7681465
  Account ID:        0x5404aee5dfc6aee1acfab0f18a7b3bf88738a1aae4778194ff627da7e7681465
  Public key (SS58): 5DxsGeocpnzgAdUAT9GpTzstTPV4MRmEDi8PDp9jyRtTjQjL
  SS58 Address:      5DxsGeocpnzgAdUAT9GpTzstTPV4MRmEDi8PDp9jyRtTjQjL
```

We now have Ed25519 key for finalizing blocks using `grandpa` for one node.

#### Generating Second Set of Keys

We will repeat the same process for the second node. We will run the following command:

```bash
./target/release/node-template key generate --scheme Sr25519 --password-interactive
```

We will be prompted to enter a password. After entering the password, we will get output similar to the following:

```log
Secret phrase:       ladder bonus finish kiss layer clock vapor voyage beauty student verb sick
  Network ID:        substrate
  Secret seed:       0xcde8fe0e8941c9985f1aae95f4b8313d950c9bc8572d578c71511b0869265f60
  Public key (hex):  0x7e7582521e85607dcc1efe5f759093a140bd2dee499d1124ea19eb618a5ddf01
  Account ID:        0x7e7582521e85607dcc1efe5f759093a140bd2dee499d1124ea19eb618a5ddf01
  Public key (SS58): 5EvWoF7sAEqLJRd9gmr2haUCUa8XVsPR594H9tymL16PBM2U
  SS58 Address:      5EvWoF7sAEqLJRd9gmr2haUCUa8XVsPR594H9tymL16PBM2U
```

This will be our `aura` key for the second node. We will need the `Secret phrase` for the next step.

```bash
./target/release/node-template key inspect --password-interactive --scheme Ed25519 "ladder bonus finish kiss layer clock vapor voyage beauty student verb sick"
```

We will be prompted to enter the password. After entering the password, we will get output similar to the following:

```log
Secret phrase:       ladder bonus finish kiss layer clock vapor voyage beauty student verb sick
  Network ID:        substrate
  Secret seed:       0xcde8fe0e8941c9985f1aae95f4b8313d950c9bc8572d578c71511b0869265f60
  Public key (hex):  0xd353bbdf98e9f68d1cd6dac8d03fa7ffff314fe6e81a9a4fa0af51568ffb8545
  Account ID:        0xd353bbdf98e9f68d1cd6dac8d03fa7ffff314fe6e81a9a4fa0af51568ffb8545
  Public key (SS58): 5GqnqPAU6PZJ8xiXTgcE81XLFJASEy14Kh29CcMt1GdUcQfa
  SS58 Address:      5GqnqPAU6PZJ8xiXTgcE81XLFJASEy14Kh29CcMt1GdUcQfa
```

This will be our `grandpa` key for the second node.

### Creating Chain Specification

We will create `customSpec.json` to share our custom chain specification with participants called validators.

Instead of creating the file from scratch, we will modify the predefined local chain specification.

To export the local chain specification, we can run the following command:

```bash
./target/release/node-template build-spec --disable-default-bootnode --chain local > customSpec.json
```

This command will generate `customSpec.json` file in the current directory.

In the `customSpec.json` file, we will modify the `name`, `ìd`, `aura` and `grandpa` fields.

```json
{
    "name": "Bootcamp Final Case",
    "id": "bootcamp_final_case",
    ...
    "genesis": {
        ...
        "aura": {
            "authorities": [
                "5D5ASJU6hUc2oJ8hmZ1xS28xEqxAtfbL1wFqkjvKCnKppZjV",
                "5EvWoF7sAEqLJRd9gmr2haUCUa8XVsPR594H9tymL16PBM2U"
            ]
        },
        "grandpa": {
            "authorities": [
                ["5DxsGeocpnzgAdUAT9GpTzstTPV4MRmEDi8PDp9jyRtTjQjL", 1],
                ["5GqnqPAU6PZJ8xiXTgcE81XLFJASEy14Kh29CcMt1GdUcQfa", 1]
            ]
        },
    }
}
```

We used our generated keys to fill the `aura` and `grandpa` fields. One from the first node and one from the second node. We also added the `name` and `id` fields to make it easier to identify our chain.

Now we have to convert our `customSpec.json` file to `customSpecRaw.json` file to be able to use it in our node. To do that, we can run the following command:

```bash
./target/release/node-template build-spec --chain=customSpec.json --raw --disable-default-bootnode > customSpecRaw.json
```

This command will generate `customSpecRaw.json` file in the current directory.

### Testing the Node

We will run the first node with the following command:

```bash
./target/release/node-template --base-path /tmp/node01 --chain ./customSpecRaw.json --port 30333 --rpc-port 9933 --no-telemetry --validator --rpc-methods Unsafe --name BootcampFinalCase01 --password-interactive
```

This command will prompt us to enter a password. After entering the password, we will get output similar to the following:

```log
...
2023-09-03 16:47:17 📋 Chain specification: Bootcamp Final Case
2023-09-03 16:47:17 🏷  Node name: BootcampFinalCase01
2023-09-03 16:47:17 👤 Role: AUTHORITY
2023-09-03 16:47:17 💾 Database: RocksDb at /tmp/node01/chains/bootcamp_final_case/db/full
2023-09-03 16:47:17 🔨 Initializing Genesis block/state (state: 0xf256…9180, header-hash: 0xc263…7fe6)
2023-09-03 16:47:17 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2023-09-03 16:47:18 Using default protocol ID "sup" because none is configured in the chain specs
2023-09-03 16:47:18 🏷  Local node identity is: 12D3KooWGU6cUDY41FtAqwCBBF5BRooCk4aUX3h6LwqNjuDcoYAM
...
2023-09-03 16:47:18 📦 Highest known block at #0
2023-09-03 16:47:18 〽️ Prometheus exporter started at 127.0.0.1:9615
2023-09-03 16:47:18 Running JSON-RPC server: addr=127.0.0.1:9933, allowed origins=["http://localhost:*", "http://127.0.0.1:*", "https://localhost:*", "https://127.0.0.1:*", "https://polkadot.js.org"]
2023-09-03 16:47:23 💤 Idle (0 peers), best: #0 (0xc263…7fe6), finalized #0 (0xc263…7fe6), ⬇ 0 ⬆ 0
```

### Adding Keys to the Keystore

#### First Node

We will add our `aura` to the keystore of the first node. We will run the following command:

```bash
./target/release/node-template key insert --base-path /tmp/node01 --chain ./customSpecRaw.json --key-type aura --scheme Sr25519 --suri "apology stairs vacuum glad awake survey universe employ use fiscal focus assist" --password-interactive
```

To add the `grandpa` key to the keystore of the first node, we will run the following command:

```bash
./target/release/node-template key insert --base-path /tmp/node01 --chain ./customSpecRaw.json --key-type gran --scheme Ed25519 --suri "apology stairs vacuum glad awake survey universe employ use fiscal focus assist" --password-interactive
```

To check if the keys are added to the keystore, we can run the following command:

```bash
ls /tmp/node01/chains/bootcamp_final_case/keystore
```

We will get output similar to the following:

```log
617572612c95b595b683c075875f594159884ece7b75d36cdc43266d825a9dd0cccc2362
6772616e5404aee5dfc6aee1acfab0f18a7b3bf88738a1aae4778194ff627da7e7681465
```

#### Second Node

We will add our `aura` to the keystore of the second node. We will run the following command:

```bash
./target/release/node-template key insert --base-path /tmp/node02 --chain ./customSpecRaw.json --key-type aura --scheme Sr25519 --suri "ladder bonus finish kiss layer clock vapor voyage beauty student verb sick" --password-interactive
```

To add the `grandpa` key to the keystore of the second node, we will run the following command:

```bash
./target/release/node-template key insert --base-path /tmp/node02 --chain ./customSpecRaw.json --key-type gran --scheme Ed25519 --suri "ladder bonus finish kiss layer clock vapor voyage beauty student verb sick" --password-interactive
```

To check if the keys are added to the keystore, we can run the following command:

```bash
ls /tmp/node02/chains/bootcamp_final_case/keystore
```

We will get output similar to the following:

```log
617572617e7582521e85607dcc1efe5f759093a140bd2dee499d1124ea19eb618a5ddf01
6772616ed353bbdf98e9f68d1cd6dac8d03fa7ffff314fe6e81a9a4fa0af51568ffb8545
```

### Running the Nodes

We will run the **first node** with the following command:

```bash
./target/release/node-template --base-path /tmp/node01 --chain ./customSpecRaw.json --port 30333 --rpc-port 9944 --no-telemetry --validator --rpc-methods Unsafe --name BootcampFinalCase01 --password-interactive
```

We will be prompted to enter a password. After entering the password, we will get output similar to the following:

```log
...
2023-09-03 17:18:29 📋 Chain specification: Bootcamp Final Case
2023-09-03 17:18:29 🏷  Node name: BootcampFinalCase01
2023-09-03 17:18:29 👤 Role: AUTHORITY
2023-09-03 17:18:29 💾 Database: RocksDb at /tmp/node01/chains/bootcamp_final_case/db/full
2023-09-03 17:18:29 🔨 Initializing Genesis block/state (state: 0xf256…9180, header-hash: 0xc263…7fe6)
2023-09-03 17:18:29 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2023-09-03 17:18:29 Using default protocol ID "sup" because none is configured in the chain specs
2023-09-03 17:18:29 🏷  Local node identity is: 12D3KooWBD2ef2GYLiikefd37D73a8Ve7UHo2w1MjmJ3pkxPZcn5
...
2023-09-03 17:18:34 💤 Idle (0 peers), best: #0 (0xc263…7fe6), finalized #0 (0xc263…7fe6), ⬇ 0 ⬆ 0
2023-09-03 17:18:39 discovered: 12D3KooWSRr6mDMt9ChZWZdVJjQgXHioBnBJbWhj4kLsGfaTGawr /ip4/127.0.0.1/tcp/30334
2023-09-03 17:18:39 💤 Idle (0 peers), best: #0 (0xc263…7fe6), finalized #0 (0xc263…7fe6), ⬇ 0 ⬆ 0
2023-09-03 17:18:42 ✨ Imported #1 (0xa624…7175)
2023-09-03 17:18:44 💤 Idle (1 peers), best: #1 (0xa624…7175), finalized #0 (0xc263…7fe6), ⬇ 1.1kiB/s ⬆ 1.0kiB/s
2023-09-03 17:18:48 🙌 Starting consensus session on top of parent 0xa624344bcf5b11d9aa1e13675f70efb20a433c945fbb52167d1860dfb6167175
2023-09-03 17:18:48 🎁 Prepared block for proposing at 2 (2 ms) [hash: 0xe995f940f207d505cd2f9197458afbf59fbe0db338f3cc084a81b080aa4583fd; parent_hash: 0xa624…7175; extrinsics (1): [0xaa46…81fa]
2023-09-03 17:18:48 🔖 Pre-sealed block for proposal at 2. Hash now 0xe6eb3fa76f42fb5a0cc26825b49293ce5db47bbea1444c151a3879b299280b20, previously 0xe995f940f207d505cd2f9197458afbf59fbe0db338f3cc084a81b080aa4583fd.
```

After that, we will run the **second node** with the following command:

```bash
./target/release/node-template --base-path /tmp/node02 --chain ./customSpecRaw.json --port 30334 --rpc-port 9945 --no-telemetry --validator --rpc-methods Unsafe --name BootcampFinalCase02 --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWBD2ef2GYLiikefd37D73a8Ve7UHo2w1MjmJ3pkxPZcn5 --password-interactive
```

You will be prompted to enter a password. After entering the password, you will get output similar to the following:

```log
...
2023-09-03 17:18:38 📋 Chain specification: Bootcamp Final Case
2023-09-03 17:18:38 🏷  Node name: BootcampFinalCase02
2023-09-03 17:18:38 👤 Role: AUTHORITY
2023-09-03 17:18:38 💾 Database: RocksDb at /tmp/node02/chains/bootcamp_final_case/db/full
2023-09-03 17:18:38 🔨 Initializing Genesis block/state (state: 0xf256…9180, header-hash: 0xc263…7fe6)
2023-09-03 17:18:38 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2023-09-03 17:18:38 Using default protocol ID "sup" because none is configured in the chain specs
2023-09-03 17:18:39 🏷  Local node identity is: 12D3KooWSRr6mDMt9ChZWZdVJjQgXHioBnBJbWhj4kLsGfaTGawr
...
2023-09-03 17:18:39 discovered: 12D3KooWBD2ef2GYLiikefd37D73a8Ve7UHo2w1MjmJ3pkxPZcn5 /ip4/127.0.0.1/tcp/30333
2023-09-03 17:18:42 🙌 Starting consensus session on top of parent 0xc263323a33c20327e66a8f2217e300ec183fb1b2eb9b849475211e1250667fe6
2023-09-03 17:18:42 🎁 Prepared block for proposing at 1 (3 ms) [hash: 0xc2e52b07946142c417e0ca23e242240146a3cf49ad93140c9f21e5948c83325c; parent_hash: 0xc263…7fe6; extrinsics (1): [0x7dc8…5a2a]
2023-09-03 17:18:42 🔖 Pre-sealed block for proposal at 1. Hash now 0xa624344bcf5b11d9aa1e13675f70efb20a433c945fbb52167d1860dfb6167175, previously 0xc2e52b07946142c417e0ca23e242240146a3cf49ad93140c9f21e5948c83325c.
2023-09-03 17:18:42 ✨ Imported #1 (0xa624…7175)
2023-09-03 17:18:44 💤 Idle (1 peers), best: #1 (0xa624…7175), finalized #0 (0xc263…7fe6), ⬇ 0.9kiB/s ⬆ 1.0kiB/s
```

## Conclusion

As we can see in the logs of the nodes, they have successfully discovered each other and started producing blocks.

In this project, we learned how to control which nodes can join our network. We also learned how to add keys to the keystore and run the nodes.
