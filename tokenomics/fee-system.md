# Fee System

The Acki Nacki blockchain uses a freemium model for transaction payments. In most cases, you can perform transactions, interact with smart contracts, store, and transfer data **without fees**.

All accounts in the Acki Nacki network are identified by a [**Dapp ID**](../glossary.md#dapp-id)**.**

## Free Transactions

**All transactions between contracts within the same** [**thread**](../glossary.md#thread) **are completely free**, without any restrictions, regardless of their association with any specific Dapp ID.\
Additionally, **data storage is free for active accounts**.

As a Dapp gains popularity, the number of transactions between its contracts will increase, and the network will allocate some accounts to a separate thread.

Transactions will only become paid if they are initiated by messages sent between threads. However, even in this case, you can make them free for your users by using the contract **auto-replenishment system**.

To do this, you need to deploy a special contract called [`DappConfig`](https://github.com/ackinacki/ackinacki/blob/main/contracts/dappconfig/DappConfig.sol), which will manage the shared balance of the Dapp.

{% hint style="info" %}
`DappConfig` can be deployed at any time. It is deployed **once per Dapp ID**.
{% endhint %}

Then, top up its balance with [SHELL](../glossary.md#shell) tokens and add balance verification and token minting logic to the contracts of your Dapp ID, using the TVM instruction `gosh.mintshell` (as shown in the [guide](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment#centralized-replenishment-of-contracts-within-a-dapp-id)). When the balance of any contract in your Dapp ID falls below the minimum you set, SHELL tokens will be debited from the `DappConfig` contract, and [VMSHELL](../glossary.md#vmshell) tokens (at a 1:1 ratio) will be minted and credited to the target contract's balance. This will be the limit within which contracts can mint VMSHELL tokens in this Dapp ID.

{% hint style="info" %}
`DappConfig` contracts have no owner, meaning any user can contribute to their balance.
{% endhint %}

**How It Works:**

During block assembly, the [**Block Keeper (BK)**](../glossary.md#block-keeper-bk) collects information on all calls to the `gosh.mintshell` TVM instruction within the transactions included in the block. For each instruction call:

1. The Dapp ID of the contract is determined.
2. The presence of a `DappConfig` contract for that Dapp ID is verified.
3. The total amount of tokens specified in the instruction calls is deducted from the `DappConfig` contract’s balance.
4. The corresponding amount of VMSHELL tokens is credited to the balances of the contracts where the instruction was invoked.

The full guide for Dapp ID is available [here](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment).

## Types of Fees&#x20;

Transaction fees is a sum of a few types of different fees connected to the execution of a single transaction. Transactions itself are complex processes, and fees are paid relative to different stages of executing them.

**Storage Fee** – Charged for data storage on the blockchain if an account has been inactive for more than 30 days.

**Compute Fee** – Paid for executing operations within smart contracts. Each operation consumes a specific amount of gas.

**Action Fee** — Fee for processing the list of actions received after the execution of the smart contract code.

**Inbound External Message Fee** - Fee for importing incoming external messages into the blockchain

**Outbound Internal Message Fee** - The amount of import charges for outgoing internal messages

For more details, refer to the [Gas Calculation Basics](https://dev.ackinacki.com/~/changes/35/gas-calculation-basics) section.

{% hint style="info" %}
Fees are paid in VMSHELL tokens.
{% endhint %}

## Fee Payment Process

### **Storage Fee**

* Charged only if an account has been inactive for more than **30 days**.
* Paid from the `DappConfig` contract’s balance.
* If the `DappConfig` contract lacks sufficient VMSHELL tokens, the missing amount is deducted from the account balance.
* If the account lacks sufficient VMSHELL tokens, the required amount is converted from SHELL tokens.
* If the account lacks sufficient funds, it will be **frozen** until the balance is replenished.

### **Compute, Action, Inbound External and Outbound Internal Message Fees**

These fees are deducted when messages are transferred between contracts in different threads.

### Fee deduction

When transferring messages between contracts under the same Dapp ID, in different threads, the fees are distributed as follows:

* **For outgoing messages:** The fee is deducted from the sender’s balance.
* **For relaying messages:** The fee is taken from either the sender’s balance or the message balance (`msg.value`), depending on the transmission flags (see [detailed documentation](https://github.com/gosh-sh/TVM-Solidity-Compiler/blob/master/API.md#addresstransfer)). The remaining amount of VMSHELL specified in `msg.value` will be credited to the recipient's balance.
* **For incoming messages:** The fee is deducted from `msg.value`, and if `tvm.accept()` is used, also from the recipient’s balance.

When transferring messages between contracts under different Dapp IDs in different threads, the entire amount of tokens specified in `msg.value` (VMSHELL) is **nullified**. In this case, the recipient contract must assume responsibility for executing the initiated transaction by calling `tvm.accept()` within the invoked function. Otherwise, the transaction will fail with the error `Not enough funds`.
