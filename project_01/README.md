# Hands On Project - 01

## Introduction

In this project, we will be building our own blockchain node using Substrate and Rust. We will also be building a frontend to interact with our blockchain.

## Prerequisites

- [Rust](https://www.rust-lang.org/tools/install)
- [Substrate](https://substrate.dev/docs/en/knowledgebase/getting-started/#manual-installation)
- [Node.js](https://nodejs.org/en/download/)
- [Yarn](https://classic.yarnpkg.com/en/docs/install/#mac-stable)

## Setup

### Installing Rust

To install Rust, we need to run the following command:

```bash
curl https://sh.rustup.rs -sSf | sh
```

Or we can use APT (Advanced Package Tool) to install Rust.

```bash
sudo apt install cargo -y
```

[!IMPORTANT]
It is recommended to install Rust using `rustup` in the official documentation.

### Substrate Node Template

First, we need to clone the Substrate Node Template.

```bash
git clone https://github.com/substrate-developer-hub/substrate-node-template
```

After cloning the repository, we need to create a new branch to compare our project with the original template.

```bash
git switch -c $OUR_NEW_BRANCH
```

To build the project, we need to run the following command:

```bash
cd substrate-node-template
cargo build --release
```

When the build is finished, we can run the node with the following command.

The `--dev` flag is used to run the node in development mode. Which means that the node will reset its state every time it is restarted.

```bash
./target/release/node-template --dev
```

Result should look like this:

```log
2023-09-02 12:34:57 🔨 Initializing Genesis block/state (state: 0xcb7f…23a8, header-hash: 0xf856…d957)
2023-09-02 12:34:57 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2023-09-02 12:34:57 Using default protocol ID "sup" because none is configured in the chain specs
2023-09-02 12:34:57 🏷  Local node identity is: 12D3KooWSbi12Mxi7ABz1DSWCHdPCzonMmgJYDwZCkbNefpzNjXX
...
2023-09-02 12:34:57 〽️ Prometheus exporter started at 127.0.0.1:9615
2023-09-02 12:34:57 Running JSON-RPC server: addr=127.0.0.1:9944, allowed origins=["*"]
```

### Substrate Frontend Template

Now, we need to clone the Substrate Frontend Template to interact with our node.

```bash
git clone https://github.com/substrate-developer-hub/substrate-front-end-template
```

After cloning the repository, we need to install the dependencies using `yarn`.

```bash
cd substrate-front-end-template
yarn install
```

To start the frontend, we need to run the following command:

```bash
yarn start
```

Result should look like this:

```log
Compiled successfully!

You can now view substrate-front-end-template in the browser.

  Local:            http://localhost:8000/substrate-front-end-template
  On Your Network:  http://127.0.0.1:8000/substrate-front-end-template

Note that the development build is not optimized.
To create a production build, use yarn build.

...

cached modules 6.64 MiB (javascript) 1.66 MiB (asset) 28.7 KiB (runtime) [cached] 2341 modules
webpack 5.76.1 compiled successfully in 2600 ms
```

Frontend is now available at `http://localhost:8000/substrate-front-end-template`. When we open the URL in our browser, we should see the following page:

![Frontend](./src/screenshots/substrate-frontend-initial_state.jpeg)

## Interacting with the Node

### Testing the Frontend

To test the frontend, we need to open the following URL in our browser:

```url
http://localhost:8000/substrate-front-end-template
```

In the "**Balances**" section, we can see the accounts that are available in our node and their balances.

![Balances](./src/screenshots/balances-initial_state.png)

We can also transfer funds between accounts. To do so, we need to select the account that we want to transfer funds from, and the account that we want to transfer funds to. Then, we need to enter the amount of funds that we want to transfer and click on the "**Submit**" button.

![Transfer-Alice_to_Dave](./src/screenshots/transfer-alice_to_dave.png)

If transaction is successful, we should see the following message under the submit button:

```log
❤️️ Transaction successful! 
tx hash: 0x10c2864e22d68412cb982d87dc3405625c25e1072d9327518d9494c634c69071, 
Block hash: 0xa7bbf16f73b6936c3c8ca050b33c6089293c7268ecc02263e9045ca12116df92
```

We can also check the "**events**" section to see the details of the transaction.

![Events-Transfer-Alice_to_Dave](./src/screenshots/events-transfer-alice_to_dave.png)

After the transaction is successful, we can see that the balance of the accounts has changed.

| Name | Address | Balance |
| ---- | ------- | ------- |
| Alice | 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY | 1,152,886,504,297,866,829 |
| Dave | 5DAAnrj7VHTznn2AWBemMuyBwZWs6FNFjdyVXUeYum3PTXFy | 35,000,000,000,000 |
