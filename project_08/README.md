# Hands on Project - 08

## Introduction

In this project, we will be building a basic smart contract using ink!.

## Prerequisites

- [rust-src](https://rust-lang.github.io/rustup/concepts/components.html)
- [cargo-contract](https://docs.rs/crate/cargo-contract/latest)
- [substrate-contracts-node](https://github.com/paritytech/substrate-contracts-node)

Other presequisites are the same as in the previous projects. You can find them [here](../project_01/README.md#prerequisites).

## Setup

### Installing the Prerequisites

#### Installing Cargo Contract

We will start by installing the `cargo-contract`. This tool is a  CLI tool which helps you develop smart contracts in Parity's ink!. What is ink!? ink! is an eDSL (embedded Domain Specific Language) for writing smart contracts on the Substrate blockchain framework.

You will need to install the `rust-src` component for your Rust toolchain. First we will add the `rust-src` component to our toolchain.

```bash
rustup component add rust-src
```

Then we can install the `cargo-contract` with the following command:

```bash
cargo install --force --locked cargo-contract
```

#### Installing Substrate Contracts Node

We will install the `substrate-contracts-node` with the following command:

```bash
cargo install contracts-node --git https://github.com/paritytech/substrate-contracts-node.git --tag v0.31.0 --force --locked
```

We should see the following output:

```log
Installing ~/.cargo/bin/substrate-contracts-node
   Installed package `contracts-node v0.31.0 (https://github.com/paritytech/substrate-contracts-node.git?tag=v0.31.0#c8863fe0)` (executable `substrate-contracts-node`)
```

To verify that the installation was successful, we can run the following command:

```bash
$ substrate-contracts-node --version
substrate-contracts-node 0.31.0-c8863fe08b7
```

### Creating New Smart Contract Project

We will create a new smart contract project with the following command:

```bash
cargo contract new flipper
```

This command will create a new folder named `flipper` and initialize a new smart contract project in it.

Now we can navigate to the `flipper` folder and test the smart contract.

```bash
cd flipper
cargo test
```

If everything went smoothly, we should see the following output:

```log
...
Compiling flipper v0.1.0 (./project_08/flipper)
    Finished test [unoptimized + debuginfo] target(s) in 35.77s
     Running unittests lib.rs (target/debug/deps/flipper-7a00414ff9f951d3)

running 2 tests
test flipper::tests::it_works ... ok
test flipper::tests::default_works ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

After we have tested the smart contract, we can build it with the following command:

```bash
cargo build
```

We should see the following output:

```log
 Compiling flipper v0.1.0 (./project_08/flipper)
    Finished dev [unoptimized + debuginfo] target(s) in 1.20s
```

### Starting the Contracts Node

We will start the contracts node with the following command:

```bash
substrate-contracts-node --log info,runtime::contracts=debug 2>&1
```

What this command does is that it starts the contracts node and redirects the logs to the terminal.

We should see the following output:

```log
2023-09-05 18:46:42.564  INFO main sc_cli::runner: Substrate Contracts Node
2023-09-05 18:46:42.564  INFO main sc_cli::runner: ✌️  version 0.31.0-c8863fe08b7
2023-09-05 18:46:42.564  INFO main sc_cli::runner: ❤️  by Parity Technologies <admin@parity.io>, 2021-2023
2023-09-05 18:46:42.564  INFO main sc_cli::runner: 📋 Chain specification: Development
2023-09-05 18:46:42.564  INFO main sc_cli::runner: 🏷  Node name: wiry-humor-1933
2023-09-05 18:46:42.564  INFO main sc_cli::runner: 👤 Role: AUTHORITY
2023-09-05 18:46:42.564  INFO main sc_cli::runner: 💾 Database: ParityDb at /tmp/substrateECZHM6/chains/dev/paritydb/full
2023-09-05 18:46:43.103  INFO main sc_service::client::client: 🔨 Initializing Genesis block/state (state: 0x1a06…77ee, header-hash: 0xb40e…a00b)
2023-09-05 18:46:43.107  INFO main sub-libp2p: 🏷  Local node identity is: 12D3KooWDRuaM9HUqTJjKwgxYYwqwoxczTSJS9fLyREjaFdGkeYb
...
2023-09-05 18:46:43.705  INFO main sc_service::builder: 📦 Highest known block at #0
2023-09-05 18:46:43.708  INFO main sc_rpc_server: Running JSON-RPC server: addr=127.0.0.1:9944, allowed origins=["*"]
2023-09-05 18:46:43.708  INFO tokio-runtime-worker substrate_prometheus_endpoint: 〽️ Prometheus exporter started at 127.0.0.1:9615
2023-09-05 18:46:48.709  INFO tokio-runtime-worker substrate: 💤 Idle (0 peers), best: #0 (0xb40e…a00b), finalized #0 (0xb40e…a00b), ⬇ 0 ⬆ 0
```

### Deploying the Smart Contract

We will head over to the [flipper contract](./flipper/) and deploy it to our local node.

```bash
cargo contract instantiate --constructor new --args "false" --suri //Alice --salt $(date +%s)
```

> [!IMPORTANT]
> If you get an error like `Failed to find any contract artifacts in target directory.`, try running `cargo contract build --release` and then try deploying the smart contract again.

We should see the following output:

```log
Dry-running new (skip with --skip-dry-run)
    Success! Gas required estimated at Weight(ref_time: 273238826, proof_size: 16689)
Confirm transaction details: (skip with --skip-confirm)
 Constructor new
        Args false
   Gas limit Weight(ref_time: 273238826, proof_size: 16689)
Submit? (Y/n):
```

We can confirm the transaction by typing `Y` and pressing `Enter`.

After the transaction is confirmed, we should see the following output:

```log
Code hash 0xaed25aff6614c3fba79002bf04783182faf67e08c19d80222c2d992aeb3557e2
    Contract 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
      Events
       Event Balances ➜ Withdraw
         who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         amount: 2.351441929mUNIT
       Event Balances ➜ Reserved
         who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         amount: 207.415mUNIT
       Event Contracts ➜ CodeStored
         code_hash: 0xaed25aff6614c3fba79002bf04783182faf67e08c19d80222c2d992aeb3557e2
       Event System ➜ NewAccount
         account: 5H8ai4YbSg7HwadHUE3NgFfdA5X3fKjjGRLYEz9d7h7mQdkD
       Event Balances ➜ Endowed
         account: 5H8ai4YbSg7HwadHUE3NgFfdA5X3fKjjGRLYEz9d7h7mQdkD
         free_balance: 100.765mUNIT
       Event Balances ➜ Transfer
         from: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         to: 5H8ai4YbSg7HwadHUE3NgFfdA5X3fKjjGRLYEz9d7h7mQdkD
         amount: 100.765mUNIT
       Event System ➜ NewAccount
         account: 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
       Event Balances ➜ Endowed
         account: 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
         free_balance: 1mUNIT
       Event Balances ➜ Transfer
         from: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         to: 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
         amount: 1mUNIT
       Event Contracts ➜ Instantiated
         deployer: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         contract: 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
       Event Balances ➜ Transfer
         from: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         to: 5H8ai4YbSg7HwadHUE3NgFfdA5X3fKjjGRLYEz9d7h7mQdkD
         amount: 100.005mUNIT
       Event TransactionPayment ➜ TransactionFeePaid
         who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         actual_fee: 2.351441929mUNIT
         tip: 0UNIT
       Event System ➜ ExtrinsicSuccess
         dispatch_info: DispatchInfo { weight: Weight { ref_time: 2351440377, proof_size: 9095 }, class: Normal, pays_fee: Yes }
```

In the local node's logs, we should see the following output:

```log
2023-09-05 18:53:26.632  INFO tokio-runtime-worker sc_basic_authorship::basic_authorship: 🙌 Starting consensus session on top of parent 0xb40e0e4a66d81c128e9693fa676e2b20ef9487d9217304cf7ff5e0285421a00b
2023-09-05 18:53:26.637  INFO tokio-runtime-worker sc_basic_authorship::basic_authorship: 🎁 Prepared block for proposing at 1 (2 ms) [hash: 0xdfe7891c5de26e01962539bb7069607307fdb8327eb7324e5c7a4f7464b14506; parent_hash: 0xb40e…a00b; extrinsics (2): [0x1e0a…1d03, 0x6298…3754]
2023-09-05 18:53:26.637  INFO tokio-runtime-worker sc_consensus_manual_seal::rpc: Consensus with no RPC sender success: CreatedBlock { hash: 0xdfe7891c5de26e01962539bb7069607307fdb8327eb7324e5c7a4f7464b14506, aux: ImportedAux { header_only: false, clear_justification_requests: false, needs_justification: false, bad_justification: false, is_new_best: true } }
2023-09-05 18:53:26.638  INFO tokio-runtime-worker substrate: ✨ Imported #1 (0xdfe7…4506)
```

This means that our smart contract is deployed successfully. We can now interact with it. To interact with the smart contract, we need to know the contract's address. We can see our contract's address in the transaction confirmation logs. It should look like this:

```log
Event Contracts ➜ Instantiated
    deployer: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
    contract: 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
```

We can see that our contract's address is `5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP`. We will use this address to interact with our smart contract.

### Interacting with the Smart Contract

We will use the `cargo contract call` command to interact with our smart contract.

```bash
cargo contract call --contract 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP --message get --suri //Alice --dry-run
```

With this command, we are calling the `get` function of our smart contract. We should see the following output:

```log
Result Success!
    Reverted false
        Data Tuple(Tuple { ident: Some("Ok"), values: [Bool(false)] })
```

Then we will call the `flip` function of our smart contract.

```bash
cargo contract call --contract 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP --message flip --suri //Alice
```

We should see the following output:

```log
Dry-running flip (skip with --skip-dry-run)
    Success! Gas required estimated at Weight(ref_time: 390262406, proof_size: 18409)
Confirm transaction details: (skip with --skip-confirm)
     Message flip
        Args
   Gas limit Weight(ref_time: 390262406, proof_size: 18409)
Submit? (Y/n): y
      Events
       Event Balances ➜ Withdraw
         who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         amount: 124.414154μUNIT
       Event Contracts ➜ Called
         caller: Signed(5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY)
         contract: 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP
       Event TransactionPayment ➜ TransactionFeePaid
         who: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
         actual_fee: 124.414154μUNIT
         tip: 0UNIT
       Event System ➜ ExtrinsicSuccess
         dispatch_info: DispatchInfo { weight: Weight { ref_time: 1331919406, proof_size: 8831 }, class: Normal, pays_fee: Yes }
```

We can see that the `flip` function is called successfully. Now we will call the `get` function again.

```bash
cargo contract call --contract 5GoBW3vYZs2yneMSVCyVLEGrTPYBWy8RDnHziomfQhhvJYKP --message get --suri //Alice --dry-run
```

We should see the following output:

```log
Result Success!
    Reverted false
        Data Tuple(Tuple { ident: Some("Ok"), values: [Bool(true)] })
```

We can see that the `get` function returns `true` now. This means that our smart contract is working as expected.

## Conclusion

In this project, we have learned how to build a basic smart contract using ink!. We have also learned how to deploy and interact with our smart contract using the `cargo contract` command and the `substrate-contracts-node`.
