---
description: This update brings together the main Acki Nacki  progress from the week
hidden: true
---

# Acki Nacki News

## Update - 15.05.26

### Node & Network

* Durable State was released in [`v0.16.0`](https://github.com/ackinacki/ackinacki/releases/tag/v0.16.0).
* Network providers are coordinating the update to keep the transition aligned across the network.

### Bridge

On the bridge side, we continue building the infrastructure for ZKP-proof verification and safe asset movement between EVM and Acki Nacki.

* The SDK now includes the `vergrth16WithVK` instruction for proving an EVM-to-Acki Nacki transfer proof on the Acki Nacki side. The instruction is not included in a public release yet.
* The next step in this direction is integrating `vergrth16WithVK` instruction into the Acki Nacki bridge and testing the full deposit to Acki Nacki scenario.
* The `Exchange` contract was renamed to `TokenBridge`.
* Withdrawal bridge interface was added to `TokenBridge`: it will allow initiating USDC withdrawals to the EVM network with event generation for a later ZKP-proof on the EVM side.
* The withdrawal bridge interface is currently under audit by the Pruvendo team.
* The circuit for layer hash movement, which proves that a block belongs to network history, was tested in an EVM test network. Based on the test results, the node-side logic for storing hash movement data needs additional refinement.

{% hint style="info" %}
There are no `Mainnet` changes yet: integration testing is running on `Shellnet`
{% endhint %}

### Dodex

For Dodex, we are continuing to develop the API and contract layer&#x20;

* The orderbook API is complete.
* The first synchronous version of the API for order creation and cancellation has been prepared. But, based on market maker scenarios, we decided to move the order creation and cancellation API to an asynchronous model to make placing large numbers of orders easier. This is a WIP.
* The API for fetching a market maker's open orders is in progress.
* Contract fixes were made based on new test results.

### Wallet

In wallet, we continue improving the user experience and adding developer tools.

* Added a `dev setting` toggle in settings to enable developer mode.
* In developer mode, users can see an option to send SHELL to a precomputed contract address for a future deployment; this option is hidden in the regular mode.
* When developer mode is enabled, the Multifactor contract address, its `DAPP ID`, and the network switcher become visible.
* The release is already available as an APK. App Store publication is coming, and Google Play publication will be later.
* Transaction history and SHELL sell order history display were fixed: the data no longer disappears in the scenarios reported by a user.
* Fixed an Android login issue for test wallets on `Shellnet`.

### Popits

We are preparing Popits 3.0 for the new boosts season.

* In the new season, users will be able to create AI tracks as Popits and receive boosts for them.
* We are also preparing a NACKL mining scenario while listening to tracks from the bucket feed or minted Popcoins.

### Documentation & Developer Experience

* A new developer documentation section on cryptography, mnemonic phrases, and keys was added: [Mnemonics and Keys](https://dev.ackinacki.com/cryptography/mnemonics-and-keys).
* A prominent [SHELL purchase](https://docs.ackinacki.com/for-users/buy-sell-shell) button was added to the [developer portal documentation](https://dev.ackinacki.com/) so users can find the token purchase flow more easily

Thanks to everyone who asks questions, reports issues, and helps make Acki Nacki clearer and more convenient for the community.

## Update - 08.05.26

This week, the team made progress across several Acki Nacki workstreams: ecosystem integrations, DEX infrastructure, Bridge, SDK, and documentation.

### Ecosystem

A PR has been opened to register Acki Nacki in the SLIP-0044 registry under number `1331`. This is an important step toward deeper integration with wallet infrastructure and ecosystem tools. \
Support our PR :rocket: [satoshilabs/slips#2016](https://github.com/satoshilabs/slips/pull/2016)

### State&#x20;

Durable state (3/4)  is in the final testing phase.&#x20;

### Bridge

This week, the Bridge team focused on reliable verification of Acki Nacki data from the EVM infrastructure side.

* The circuit for proving attestations in Acki Nacki block on the EVM network side has been successfully tested in an EVM testnet.
* Integration and testing are underway for the circuit that proves Layer Hashes movement. This mechanism confirms that a block belongs to the Acki Nacki network history.

### Dodex

Dodex continues to expand the infrastructure needed by market makers and applications to work with the DEX effectively.

#### Completed

* Finalized the DEX API specification for market makers and web applications.
* Prepared a library for writing to the Orderbook, including order creation and cancellation.
* Implemented an API for retrieving the list of Prediction Markets.

#### In progress

* Orderbook read API
* Orderbook write API for market makers.

### Documentation

In parallel, we continue improving the documentation so users, developers, and partners can better understand the ecosystem and complete key flows faster.

* Published a detailed overview of the network architecture and participant roles: \
  [Network Architecture and Participants](https://docs.ackinacki.com/network-architecture-and-participants).
* Updated the documentation for buying SHELL tokens to reflect the simplified user flow in [shellbuy.ackinax.com](https://shellbuy.ackinax.com): \
  [Purchase with crypto or card](https://docs.ackinacki.com/for-users/buy-sell-shell/buying-shell/purchase-with-crypto-or-card-credit-debit).
* Clarified the steps for obtaining `app_dapp_id` in the Bee Engine SDK integration documentation: \
  [Bee Engine SDK Integration Documentation](https://dev.ackinacki.com/bee-engine/bee-engine-sdk-integration-documentation).

Thank you to everyone following Acki Nacki's progress, supporting the team's initiatives, and helping move the ecosystem forward.
