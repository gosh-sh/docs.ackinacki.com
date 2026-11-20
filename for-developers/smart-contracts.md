# Smart Contracts

A smart contract is code that runs when its account receives a message. The account holds the contract's state and balance.

{% hint style="warning" %}
Use the [AVM SDK](https://github.com/gosh-sh/avm-sdk/blob/main/README.md) for new smart contracts. \
Acki Nacki is gradually moving away from TVM.
{% endhint %}

## Rust contracts with AVM

With the [AVM SDK](https://github.com/gosh-sh/avm-sdk/blob/main/README.md), you write a contract in Rust and compile it to WebAssembly. When a message reaches the account, AVM runs the contract's code against its current state. The contract can update that state and send messages to other accounts.

For example, the owner of the SDK's sample wallet signs a request to send funds. The wallet processes the request and sends an internal message carrying value to the recipient. You can try building and calling a Rust contract on your computer with the SDK's local stand.

[Build and call your first AVM contract](https://dev.ackinacki.com/avm-sdk/getting-started)

## Solidity contracts with TVM

{% hint style="info" icon="thumbs-up" %}
We recommend using the [AVM SDK](https://github.com/gosh-sh/avm-sdk/blob/main/README.md) for developing new smart contracts.
{% endhint %}

Solidity contracts compile to bytecode for the **TON Virtual Machine (TVM)**. Use these resources to build and deploy them.

* **TON Virtual Machine (TVM)**.
* [VM Instruction Set](https://dev.ackinacki.com/vm-instructions/acki-nacki-vm-instructions)
* [Guide to Development and Deployment](https://dev.ackinacki.com/dapp-id-full-guide-creation-fees-centralized-replenishment)
*   [ABI Specification and Data Encoding](https://github.com/tvmlabs/tvm-abi/blob/main/README.md)

    Acki Nacki uses a strict ABI (Application Binary Interface) format for:

    * Encoding and decoding input/output
    * Defining function signatures
    * Structuring on-chain communication
* [Compilation](https://github.com/gosh-sh/TVM-Solidity-Compiler)\
  Compilation transforms your Solidity code into bytecode executed by the TVM.
* Security Best Practices
* [The Acki Nacki Fee System →](https://docs.ackinacki.com/tokenomics/fee-system)
