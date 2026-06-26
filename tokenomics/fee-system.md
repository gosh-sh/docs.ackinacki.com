# Fee System

All accounts in the Acki Nacki network are identified by **Dapp ID**.

Transactions in Acki Nacki always consume network resources and therefore include protocol-level fees. **However, if a Dapp has a `DappConfig` contract, fees paid for transactions between contracts within the same thread are compensated by the built-in fee factory mechanism regardless of whether the contracts belong to a specific Dapp ID. Fees are not compensated when transactions are initiated by messages between different threads or when no `DappConfig` contract is configured for the Dapp ID.**

### Built-in Fee Factory

To enable this mechanism for your Dapp ID, deploy the special [`DappConfig`](https://github.com/ackinacki/ackinacki/blob/main/contracts/dappconfig/DappConfig.sol) contract.

`DappConfig` is **not the main contract of your Dapp**. It is a service contract that manages the Dapp credit limit and automatically replenishes its contracts.

{% hint style="info" %}
`DappConfig` can be deployed at any time. It is deployed once per Dapp ID. \
These contracts do not have an owner, so any user can top up their balance.
{% endhint %}

#### How It Works

1. You deploy `DappConfig` and top up its balance with SHELL tokens.
2. When your Dapp contracts run out of funds, they call the TVM instruction [`gosh.mintshell` or `gosh.mintshellq`](https://github.com/gosh-sh/TVM-Solidity-Compiler/blob/master/API.md#goshmintshell-and-goshmintshellq). It allows an account to receive tokens from the credit limit of its `DappConfig`, mints VMSHELL at a 1:1 ratio, and credits them to the calling contract balance (see the [guide](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment#centralized-replenishment-of-contracts-within-a-dapp-id)).
3. During block production, the **Block Keeper (BK)** processes these calls and checks whether `DappConfig` exists for the specified Dapp ID.
4. If it exists, the BK calculates how much VMSHELL was generated in the block and then subtracts from that amount the gas actually spent on internal messages inside the Dapp.
5. As a result, the `DappConfig` available limit is charged **not for the full generated amount, but only for the remainder after this adjustment**.

This is how the fee factory works: fees paid for messages within the same thread are compensated through this adjustment. For a Dapp with `DappConfig`, this mechanism allows fees to circulate inside the Dapp instead of becoming direct account expenses.

At the protocol level, transactions are paid from the balance of the account that executes the transaction, or from message funds depending on the send flags. `DappConfig` does not replace this payment source for regular transaction fees: it gives an account the ability to top up its VMSHELL balance in advance. In practice, transactions become direct expenses when they are initiated by messages between different threads or when no `DappConfig` contract is configured for the Dapp ID.

{% hint style="info" %}
System contracts use a system `DappConfig` with the `is_unlimit` flag. For this config, the available limit does not restrict minting: system contracts can take tokens from the system `DappConfig` without decreasing `available_balance`.
{% endhint %}

The full Dapp ID guide is available [here](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment).

### Automatic Replenishment

Add balance checks and a `gosh.mintshell` minting call to your Dapp ID contracts. When the balance of any contract in your Dapp ID falls below the minimum threshold you define, the contract can take tokens from the available limit of its `DappConfig`: VMSHELL is minted at a 1:1 ratio and credited to the target contract balance. `available_balance` in `DappConfig` defines the maximum SHELL credit amount within which contracts of this Dapp ID can mint VMSHELL.

{% hint style="info" %}
Developers decide for themselves how to implement automatic replenishment inside their Dapp ID.

However, note that if you replenish contracts directly without deploying `DappConfig`, your Dapp ID will not have the mechanism that allows fees to circulate indefinitely inside the system.
{% endhint %}

### Fee Types

Transaction fees are the sum of several different fees related to the execution of a single transaction. Transactions themselves are complex processes, and fees are charged for different stages of their execution.

**Storage Fee** - charged for storing data in the blockchain if the account has been inactive for more than 30 days.

**Compute Fee** - paid for executing operations inside smart contracts. Each operation consumes a certain amount of gas.

**Action Fee** - charged for processing the action list produced after smart contract code execution.

**Inbound External Message Fee** - charged for importing inbound external messages into the blockchain.

**Outbound Internal Message Fee** - the sum of import fees for outbound internal messages.

For more details, see [Gas Calculation Basics](https://dev.ackinacki.com/~/changes/35/gas-calculation-basics).

{% hint style="info" %}
Fees are paid in VMSHELL tokens.
{% endhint %}

### Fee Payment Process

In Acki Nacki, fees are paid in VMSHELL during transaction processing. The main payment source is the balance of the account that executes the transaction or sends the message. Depending on the send flags (see the [detailed documentation](https://github.com/gosh-sh/TVM-Solidity-Compiler/blob/master/API.md#addresstransfer)), part of the fees may also be withheld from `msg.value`.

`DappConfig` is used to replenish the balances of Dapp ID accounts. An account can call `gosh.mintshell/gosh.mintshellq`, receive VMSHELL from the available limit of its `DappConfig`, and then pay for the transaction from its own balance. Therefore, `DappConfig` is not the direct payer of fees: fees are deducted from the VMSHELL balance of the account that executes the transaction.

For system contracts, a system `DappConfig` with `is_unlimit = true` is used. Such contracts can mint VMSHELL from the system config without being limited by `available_balance`.

#### Storage Fee

Storage Fee is charged for storing account state if the account has been inactive for more than **30 days**. The fee is deducted from the account's own VMSHELL balance during the next transaction processing, before the contract logic is executed.

If the balance is insufficient, the available part is deducted, and the outstanding remainder is recorded as storage debt. `DappConfig` does not pay Storage Fee directly: it can only replenish the account balance in advance through `gosh.mintshell(...)` so that the account has VMSHELL to pay fees. During subsequent inbound internal messages, the debt may be repaid from incoming funds. If the debt exceeds protocol thresholds, the account may be frozen or deleted.

#### Compute Fee

Compute Fee is charged for executing contract code. The fee amount depends on the gas used. The fee is paid from the funds available for transaction execution, including the account balance and the applicable inbound message processing rules.

#### Action Fee

Action Fee is charged for executing the actions produced by the contract after the compute phase, such as sending messages, reserving funds, or changing state. These fees are paid by the account that performs the actions.

#### Inbound External Message Fee

Inbound External Message Fee is charged for importing an external message into the network and processing it by the target account. If the account does not have enough funds to process the external message, the transaction may be rejected or completed without executing code.

#### Outbound Internal Message Fees

Outbound Internal Message Fees are charged when internal messages are sent between contracts. Depending on the send flags, the fee may be paid separately from the sender balance or withheld from `msg.value`, reducing the amount received by the recipient.

For messages within the same thread, the corresponding amount may be compensated through the Block Keeper adjustment described above. For a Dapp with `DappConfig`, this makes internal activity effectively free for accounts after their balance is replenished, while messages between threads or Dapp IDs without `DappConfig` remain direct expenses.

When messages are passed between contracts with different Dapp IDs in different threads, the entire token amount specified in `msg.value` (VMSHELL) is **zeroed out**. In this case, the recipient contract must take responsibility for executing the initiated transaction by calling `tvm.accept()` inside the called function. Otherwise, the transaction fails with `Not enough funds`.
