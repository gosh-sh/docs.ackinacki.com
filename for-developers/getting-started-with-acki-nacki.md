# Getting Started with Acki Nacki

Welcome to the **Acki Nacki Developer Portal**!\
This portal will walk you through the key concepts of building on Acki Nacki — even if you're completely new to it.

Whether you're deploying your first smart contract, testing transactions, or building a full-scale Dapp, this section will help you get up and running quickly.

### 🧠 Core Concepts

In the **Acki Nacki** blockchain, every user action begins with a [**message**](messages.md).\
Messages trigger [**transactions**](transactions.md), which then modify the **state of** [**accounts**](accounts.md) **(smart contracts)**.

#### 🔐 Account = Smart Contract

Every contract is an **account** in the blockchain. It includes:

* **Balance**
* **Code**
* **State (data)**

Contracts:

* Can **own and send funds**
* Can **execute code**
* Can **create other contracts**
* Interact **asynchronously** with other contracts

All logic — user behavior, applications, business systems — lives inside these contracts.

### 🧩 Dapp ID — A Unique Application Identity

Acki Nacki introduces a unique concept called **Dapp ID** — a logical identifier that connects related contracts into one unified Dapp.

**Dapp ID is used for:**

* Grouping contracts into a single application unit
* Routing messages and managing execution priorities
* Implementing **freemium** models (subsidized transactions)
* Simplifying balance/state management across the Dapp

> All accounts on Acki Nacki are identified by a **Dapp ID** — this makes it easier to structure, scale, and monetize your Dapps.

Before diving into DAPP development, it is recommended to first understand the [basic principles of how a blockchain works](https://docs.ackinacki.com/), as well as the architecture and decentralization mechanisms of the Acki Nacki network.

🔗You can learn more about Dapp [here](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment).

### 💸 Freemium Transaction Model

Acki Nacki supports a **freemium model** for transactions:\
You can often **interact with contracts, send messages, and store data without paying fees**.

This opens the door to new UX possibilities: free onboarding, subsidized interactions, and seamless microservices.

### ⚙️ Smart Contract Development

Smart contracts on Acki Nacki are written in **Solidity**. If you’re familiar with Ethereum development, you’ll feel right at home.

🔗Check out our Solidity-based contract development [guide](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment#prepare-contract-source-code).

#### 🧪 Executing WASM binaries within a smart contract

Need to run compute-heavy operations inside your contract?

Acki Nacki supports **running precompiled WebAssembly (WASM)** code using the `RUNWASM` instruction.\
This lets you execute binaries built to the [WASI Preview 2 Component Model Standard](https://github.com/WebAssembly/component-model) directly inside the contract, powered by **Wasmtime**.

This makes Acki Nacki ideal for:

* High-performance computation
* Data science workflows
* Gaming logic
* On-chain AI modules

More details here:\
📄 [RUNWASM Instruction](https://github.com/tvmlabs/tvm-sdk/blob/main/tvm_vm/WASM.md)

### 🛠 Developer Tools & SDKs

You can interact with the blockchain using the official **Acki Nacki SDK** and developer tools.

* Perform queries, send transactions, monitor events
* Integrate GraphQL directly into your app or backend

🔍 You can learn more about Developer Tools [here](developer-tools-and-sdk.md).

### 🔗 Useful Links

| Resource              | Link                                                                     |
| --------------------- | ------------------------------------------------------------------------ |
| 🧑‍💻 GitHub          | [github.com/ackinacki/ackinacki](https://github.com/ackinacki/ackinacki) |
| 💬 Telegram           | [t.me/tvmlabs](https://t.me/tvmlabs)                                     |
| 🔍 Explorer (Mainnet) | [mainnet.ackinacki.org/graphql](https://mainnet.ackinacki.org/graphql)   |
| 🧪 Explorer (Testnet) | [shellnet.ackinacki.org/graphql](https://shellnet.ackinacki.org/graphql) |

***

### 📩 Feedback & Support

* Have a question? Need help? → [Join our Telegram](https://t.me/tvmlabs)
* Found a bug? → Submit an issue on [GitHub](https://github.com/ackinacki/ackinacki/issues)

