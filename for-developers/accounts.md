# Accounts

#### Core Principles

* **Account (Contract)** – a record in the distributed database that includes the balance, code, and data.
* All user, application, and system logic and state are stored in contracts.
* Each contract is an account capable of sending messages, executing code, holding funds, and creating other contracts.
* Contracts can interact with other contracts asynchronously.
* Accounts are identified and grouped using **Dapp ID**.

**Dapp ID** **is a unique identifier for a logical application, linking together contracts that operate within the same Dapp.**

Before diving into building a Dapp, it’s recommended to first understand the [basic principles of how blockchain works](../), as well as the architecture and decentralization mechanisms of the Acki Nacki network.

***

#### Purpose of Dapp ID:

* Represents a logical "group of accounts" (contracts) acting as a single application
* Used for message routing and execution priority management
* Enables transaction subsidization and freemium models
* Simplifies state and balance management across the entire Dapp

***

#### Dapp ID Assignment Rules:

* Every initialized and active contract has a Dapp ID.
* If a contract is created directly (via an external message), its Dapp ID matches its own address.
* If a contract is created by another contract (via an internal message), it inherits the Dapp ID of its creator.

***

You can learn how to create your first Dapp using [this guide](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment).
