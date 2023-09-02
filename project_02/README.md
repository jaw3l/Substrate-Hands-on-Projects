# Hands on Project - 02

## Introduction

In this project, we will simulate a blockchain network with two nodes and we will verify block production.

## Prerequisites

Presequisites are the same as in the previous project. You can find them [here](../project_01/README.md#prerequisites).

## Setup

### Purging Chain State

We will start by purging the chain state of the previous project. Since we used the `--dev` flag, the chain state will be reset every time we restart the node.

But if we want to purge the chain state manually and didn't use the `--dev` flag, we can do it with the following command:

```bash
./target/release/node-template purge-chain
```

This command will prompt us to confirm the purge. We can confirm it by typing `Y` and pressing `Enter`.

### Starting the Nodes

We will start two nodes with different keys. We will use the `--alice` flag for the first node and `--bob` flag for the second node. These flags are shortcuts for the `name` and `validator` keys. You can find more information in the [Substrate Documentation](https://docs.substrate.io/).

#### Alice Node

We will start a node using "Alice" account as the key.

```bash
./target/release/node-template --base-path /tmp/alice --chain local --alice --port 30333 --rpc-port 9944 --no-telemetry --node-key 0000000000000000000000000000000000000000000000000000000000000001
```

Once the node is started, we can see the logs in the terminal.
You can see our local node's **identity** in the logs. It should look like this:

```log
2023-09-02 14:20:14 Substrate Node
...
2023-09-02 14:20:14 🏷  Node name: Alice
...
2023-09-02 14:20:14 🏷  Local node identity is: 12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp
...
2023-09-02 14:20:14 📦 Highest known block at #0
...
2023-09-02 14:20:15 Accepting new connection 1/100
...
2023-09-02 14:20:34 💤 Idle (0 peers), best: #0 (0xa8b3…d6fd), finalized #0 (0xa8b3…d6fd), ⬇ 0 ⬆ 0
2023-09-02 14:20:34 discovered: 12D3KooWQwQPn8TRDBXFFR11Tn8nTJ1ZDhdc2vCBNyA8DhoyNHGS /ip4/127.0.0.1/tcp/30334
2023-09-02 14:20:36 🙌 Starting consensus session on top of parent 0xce77de15e6e5f9861b318f1133f7ffd7c231ee5742ff37a636f6d4d94aa9285c
2023-09-02 14:20:36 🎁 Prepared block for proposing at 15 (0 ms) [hash: 0x49b186e2c8fb5f2f80cff3a53475f095be9941823652649beb81391f18e8b661; parent_hash: 0xce77…285c; extrinsics (1): [0xd283…b2b7]
2023-09-02 14:20:36 🔖 Pre-sealed block for proposal at 15. Hash now 0x1ae5fa3e18a56441368984e8a7fa589675c5e869b90bce79b5d1ce63e1469b49, previously 0x49b186e2c8fb5f2f80cff3a53475f095be9941823652649beb81391f18e8b661.
2023-09-02 14:20:36 ✨ Imported #15 (0x1ae5…9b49)
```

As we can see in the logs, our local node's **identity** is `12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp`.

We can use this **identity** as the bootnode for our second node.

#### Bob Node

Now we will start a second node using "Bob" account as the key.

```bash
./target/release/node-template --base-path /tmp/bob --chain local --bob --port 30334 --rpc-port 9934 --no-telemetry --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp
```

```log
2023-09-02 14:20:34 Substrate Node
...
2023-09-02 14:20:34 🏷  Node name: Bob
...
2023-09-02 14:20:34 🏷  Local node identity is: 12D3KooWQwQPn8TRDBXFFR11Tn8nTJ1ZDhdc2vCBNyA8DhoyNHGS
...
2023-09-02 14:20:34 📦 Highest known block at #14
...
2023-09-02 14:20:34 discovered: 12D3KooWEyoppNCUx8Yx66oV9fJnriXwCcXwDDUA2kj6vnc6iDEp /ip4/127.0.0.1/tcp/30333
2023-09-02 14:20:36 ✨ Imported #15 (0x1ae5…9b49)
2023-09-02 14:20:39 💤 Idle (1 peers), best: #15 (0x1ae5…9b49), finalized #12 (0x23d9…3205), ⬇ 1.0kiB/s ⬆ 1.9kiB/s
2023-09-02 14:20:42 🙌 Starting consensus session on top of parent 0x1ae5fa3e18a56441368984e8a7fa589675c5e869b90bce79b5d1ce63e1469b49
2023-09-02 14:20:42 🎁 Prepared block for proposing at 16 (0 ms) [hash: 0xf98589c7c777b9bb4409b4bce322903c72b767747b05140860b670e388d88863; parent_hash: 0x1ae5…9b49; extrinsics (1): [0xaed2…f388]
2023-09-02 14:20:42 🔖 Pre-sealed block for proposal at 16. Hash now 0xbbb64d03f3cea28656271a4563e21ff41cc9046659ea59bcfb678d98e92ea2e1, previously 0xf98589c7c777b9bb4409b4bce322903c72b767747b05140860b670e388d88863.
2023-09-02 14:20:42 ✨ Imported #16 (0xbbb6…a2e1)
```

## Conclusion

As we can see in the logs our main node (Alice) is the bootnode for our second node (Bob). We can see that Alice node discovered Bob node and Bob node imported the block produced by Alice node. We have successfully created a blockchain network with two nodes.
