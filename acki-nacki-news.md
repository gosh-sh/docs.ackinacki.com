---
description: This update brings together the main Acki Nacki  progress from the week
hidden: true
---

# Acki Nacki News

## Update - 26.06.26

### DEX.DO

* **The** [**Points and Rewards program terms have been published**](https://seasons.dex.do/)**. The program launch is coming — stay tuned for updates!**
* [**https://app.dex.do/**](https://app.dex.do/) **dashboard and leaderboards for Points and Rewards program participants is released.**&#x20;
* Debugging of the AI Token Market client is in progress. AI Token exchange contracts are ready.
* The skills library has been expanded with a new set of DEX.DO automation and trading skills, covering Multisig setup, deposits, account registration, market data, trading, and withdrawals. Learn more: https://github.com/gosh-sh/dexdo/tree/dev/.claude/skills&#x20;

### Node & Network & SDK

* Node testing with blockchain multithreading continues. Synchronization stability has been improved, and Poseidon integration is in progress (instruction required for Bridge).&#x20;
* Published [Bee SDK](https://www.npmjs.com/package/@teamgosh/bee-sdk) on npm (`@teamgosh/bee-sdk`)

### Bridge

* The StateV2 release with multithreading is getting closer, so the team has moved to the stage of supporting multithreading in the bridge circuits.
* Pruvendo brought in additional resources to audit the EVM side of the bridge. The issues found have already been fixed, and the final version of the chain for depositing USDC into Acki Nacki will be deployed during the next Shellnet restart.

### Documentation

* Published a new user guide: [What to Do If Your Authentication Factor Has Expired](https://docs.ackinacki.com/for-users/wallets/troubleshooting/what-to-do-if-your-authentication-factor-has-expired), with step-by-step instructions for updating an expired authentication factor.

## Update - 19.06.26

### DEX.DO

* **The Points and Rewards program terms have been finalized, and the new landing page is ready for publication. Stay tuned for announcements!**
* The backend for calculating program points, the participant account, and leaderboard are in development.&#x20;
* A smart contract system with a primary market for model tokens is being developed in parallel. The program launch will not be blocked by primary market support for model tokens.
* Added an AI skill for deploying the Multisig, as well as deploying and topping up Private Notes on Shellnet: [dexdo-onboarding-shellnet](https://github.com/gosh-sh/dexdo/blob/dev/.claude/skills/dexdo-onboarding-shellnet/SKILL.md).

### Ecosystem

* The PR to add Acki Nacki to the SLIP-0044 registry has been accepted. The project has been assigned number `1331`, creating the foundation for further integration with wallets and blockchain infrastructure services.

### Node & Network & SDK

* The network has been upgraded to `v0.16.3` with support for the new addressing scheme.
* A new `bee-engine` has been published. The release includes `bee-connect` and `bee-wallet` for the first time. Usage examples are available in the [miner-react example](https://github.com/gosh-sh/bee-engine/tree/main/examples/javascript/miner-react).

### Bridge

* The latest bridge updates have been rolled out to Shellnet. Infrastructure preparation has started for calculating heavy proofs of key blocks on the EVM side. Since Mainnet BK set updates may happen as often as once every 5 minutes, proof calculation will be parallelized. This will allow the heavy part of the proof to move faster, leaving the user only to complete the lightweight event proof.

### Popit Music

* Popit Music has been updated: generation token logic has been fully migrated to GoshAI Agent Tokens, Agent Tokens can now be purchased with fiat, localization has been added for popular languages, and the track creation flow has been improved. It is now more stable and produces results closer to the user's prompt. Try the update here: [Popit Music](https://popit.music/?utm_source=community\&utm_medium=social\&utm_campaign=update_2026_06_19\&utm_content=community_update).

### Documentation

Added page: [**Connecting an Acki Nacki Wallet and Setting Up Mining Keys**](https://docs.ackinacki.com/for-users/wallets/connecting-an-acki-nacki-wallet-and-setting-up-mining-keys)

## Update - 12.06.26

### DEX.DO

* **While preparing the Points & Rewards program, we made an important product decision: DEX.DO will expand beyond prediction markets into a decentralized trading layer for AI model access. We are updating the program structure to reflect this broader product direction and reward meaningful participation across DEX.DO. More details will follow.**
* Added a public Trades API that does not require authorization, allowing retrieval of the latest market price and recent trade history.

### Node & Network & SDK

* Testing of the `v0.16.3` release with the new addressing scheme has been completed. Network upgrade announcements will be published separately.
* `bee-engine` and `ackinacki-kit` now support `SDK 3.0.0`.

### Bridge

* Published the repository for generating proofs for withdrawals from Acki Nacki: [https://github.com/gosh-sh/acki-nacki-to-eth-bridge-halo2-prover](https://github.com/gosh-sh/acki-nacki-to-eth-bridge-halo2-prover)
* Local end-to-end tests of withdrawals from Acki Nacki to EVM were successfully completed on a local network without BK set updates.
* The BK Set Update circuit has completed the first audit cycle by Pruvendo. The circuit is currently being covered with additional tests.
* The next step is to determine the optimal circuit parameters to ensure acceptable withdrawal timings to EVM.

### Apps

* Wallet have been migrated to SDK 3.0 and sent to App Store and Play Market for review.
* Popits and Batteries have been migrated to SDK 3.0. Migration of Popit Music and Ludo is planned for next week.

## Update - 05.06.26

### DEX.DO

* **The terms for the Points & Rewards incentive program are being prepared to help attract testers and participants to DEX.DO. The program terms will be published soon.**
* **Launched the public Oracle API** — a new endpoint for accessing oracle information and available events for market creation.
* An open-source release of the DEX.DO stack - including halo2 libraries, the indexer, API, and exchange integration libraries: [dexdo](https://github.com/gosh-sh/dexdo), [dexdo-halo2-kit](https://github.com/gosh-sh/dexdo-halo2-kit), [DEX.DO documentation](https://gosh-sh.github.io/dexdo/).
* A new giver has been published in Shellnet with support for parallel top-ups to speed up testing.

### Block Manager Licenses

Sales of [Block Manager Licenses](https://bm.gosh.sh/) have started. These licenses are intended for independent operators of Acki Nacki network access infrastructure.

### Popits

[Popits 3.0](https://popit.music/home) has launched together with a new Boosts season. Popits has evolved into an AI ecosystem for creating, distributing, and monetizing music: users can generate original tracks with AI, publish and mint them, and participate in community-driven contests and reward programs.

The new season includes NACKL mining mechanics linked to music listening and platform activity. At launch, more than 1,000 AI-generated tracks created by the community during closed testing were available. Popits is also available through the [Telegram bot](https://t.me/PopitMusic_Bot).

### Node & Network & SDK

* [TVM SDK 3.0.0](https://github.com/tvmlabs/tvm-sdk/releases/tag/v3.0.0.an) has been released, and the new addressing scheme has been rolled out to Shellnet. Shellnet is now compatible only with SDK 3.0.0, so application developers should migrate to this version now. The [migration guide is here](https://github.com/tvmlabs/tvm-sdk/blob/main/docs/MIGRATION-3.0.md).
* `bee-engine` and `ackinacki-kit` do not support SDK 3.0.0 yet. A release with 3.0.0 support will follow soon; stay tuned for announcements.
* The Mainnet release of the new addressing scheme is still being tested.

### Network Status

A public [Acki Nacki network status](https://grafana.ackinacki.org/) page has been launched.

### Bridge

* A repository for generating proofs for withdrawals from Acki Nacki has been prepared. It will be published soon.
* Deposit testing from a test EVM network to Acki Nacki Shellnet has been completed; a guide will be published soon. The latest version of the bridge contract with deposit support has been deployed to Shellnet.
* Work on the circuit for BK set updates has resumed.

### Wallet

AN Wallet is currently not compatible with Shellnet: to speed up development and testing, the final API version with the new addressing scheme was rolled out there first. An AN Wallet release with Shellnet support is expected in the coming days.

## Update - 29.05.26

### Node & Network & SDK

TVM-SDK 3.0.0 with new addressing scheme is coming. All the applications MUST migrate to it before StateV2 release.&#x20;

### Bridge

End-to-end positive deposit/withdraw scenarios testing is ongoing. The audit excludes BK set update proof verification, as Shellnet currently does not perform BK set updates. User documentation for transfers from the test EVM network to Shellnet is also in preparation. Stay tuned for upcoming announcements!

### DEX.DO

* Added account balance APIs covering both collateral and per-market outcome tokens.&#x20;
* Exposed maker and taker commission rates on the markets endpoint.&#x20;
* Published the OpenAPI specification.&#x20;
* Released POST `/api/v1/buyFullSet` for buying a full set of outcome tokens with collateral; \
  the matching sellFullSet and claim endpoints are specified and will follow.&#x20;

An open-source release of the DEX.DO stack - including halo2 libraries, the indexer, API, and exchange integration libraries - is coming. Stay tuned. 🚀

### Documentation

A new [Synchronization and Consensus Mechanism](https://docs.ackinacki.com/synchronization-and-consensus-mechanism) section was added. It explains how Acki Nacki reaches consensus on block order and validation, including finalization and error handling.

### Support

Support requests were simplified and now go through a single bot: [@ackinackisupport\_bot](https://t.me/ackinackisupport_bot). A questionnaire and screenshot upload option were added. Bot links are available in all official chats, and requests are reviewed daily.

We established a new automated process of collecting feedback from the community across all chat channels for further discussion and follow-up with the core team.

## Update - 22.05.26

### Node & Network

After the Durable State release, the network team identified a bug in external message processing. Patch releases [`v0.16.1`](https://github.com/ackinacki/ackinacki/releases/tag/v0.16.1) and [`v0.16.2`](https://github.com/ackinacki/ackinacki/releases/tag/v0.16.2) were prepared and have already been rolled out to the network.

Work on State V2 is currently in progress.

{% hint style="danger" %}
**Attention!!!!!**

Account addressing in State V2 will use the DApp ID + address format. At the moment, all applications are still using the old addressing scheme.

We are planning to prepare a migration guide for applications to support the transition.

The SDK with new account addressing support is already available (latest version: [v2.24.21](https://github.com/tvmlabs/tvm-sdk/releases/tag/v2.24.21.an))
{% endhint %}

### API Access

Degraded public API performance over the last 1.5 week was caused by increased miner bot activity. These bots abused public infrastructure from a set of IP addresses.

To prevent this load from affecting other network users, the rate limit policy for public endpoints was tightened. API request optimizations were also made to improve response speed.

Miner applications may acquire a Block Manager license to avoid these limits. There will be an additional post on this topic the next week.&#x20;

<mark style="color:$success;">**The internal network economy is evolving, and that is great to see!**</mark>

### Bridge

Changes made to the TokenBridge event circuit based on the results of the Pruvendo audit.

Decision was made to replace `vergrth16WithVK` instruction with `ZKHALO2VERIFYWITHVK` - this will simplify the integration a lot.&#x20;

### DODEX

Dodex development focused on the Market Maker API and Shellnet testing tools. Added Cancel Order API, all orders API (including cancelled, rejected and fulfilled orders), and batched API support. Prepared the first version of scripts for producing artificial load for end-to-end API testing on Shellnet.

Maker and Taker commission supported in DODEX core smart contracts.

### Wallet

A wallet release with support for the new DAPP ID + Account ID addressing format is planned in the near future. Please follow upcoming announcements.\
After the State V2 release, the old addressing format will no longer be supported.\
We will keep you updated.

### Documentation & Developer Experience

A separate documentation page was added for receiving test tokens on Shellnet: [Get test tokens from giver in Shellnet](https://dev.ackinacki.com/readme/get-test-tokens-from-giver-in-shellnet).&#x20;

## Update - 15.05.26

### Node & Network

* Durable State was released in [`v0.16.0`](https://github.com/ackinacki/ackinacki/releases/tag/v0.16.0). Network providers are coordinating the update to keep the transition aligned across the network.

### Bridge

On the bridge side, we continue building the infrastructure for ZKP-proof verification and safe asset movement between EVM and Acki Nacki.

* The SDK now includes the `vergrth16WithVK` instruction for proving an EVM-to-Acki Nacki transfer proof on the Acki Nacki side. The instruction is not included in a public release yet. The next step in this direction is integrating `vergrth16WithVK` instruction into the Acki Nacki bridge and testing the full deposit to Acki Nacki scenario.
* The `Exchange` contract was renamed to `TokenBridge`  and withdrawal bridge interface was added to `TokenBridge`: it will allow initiating USDC withdrawals to the EVM network with event generation for a later ZKP-proof on the EVM side. The withdrawal bridge interface is currently under audit by the Pruvendo team. These changes are in Shellnet only, not in Mainnet.
* The circuit for layer hash movement, which proves that a block belongs to network history, was tested in an EVM test network. Based on the test results, the node-side logic for storing hash movement data needs additional refinement.

{% hint style="info" %}
There are no `Mainnet` changes yet: integration testing is running on `Shellnet`
{% endhint %}

### DODEX

For Dodex, we are continuing to develop the API and contract layer&#x20;

* The orderbook API is complete.
* The first synchronous version of the API for order creation and cancellation has been prepared. But, based on market maker scenarios, we decided to move the order creation and cancellation API to an asynchronous model to make placing large numbers of orders easier. This is a WIP.
* The API for fetching a market maker's open orders is in progress.
* Contract fixes were made based on new test results.

### Wallet

In wallet, we continue improving the user experience and adding developer tools.

* In the coming release  `Dev setting`  page is added
* When developer mode is enabled,&#x20;
  * the Multifactor contract address, its `DAPP ID`, and the network switcher become visible.
  * users can see an option to send SHELL to a precomputed contract address for a future deployment; this option is hidden in the regular mode.
* The release is already available as an APK. App Store publication is coming, and Google Play publication will be later.
* Transaction history and SHELL sell order history display were fixed: the data no longer disappears in the scenarios reported by a user.
* Fixed an Android login issue for test wallets on `Shellnet`.

### Popits

We are preparing Popits 3.0 for the new boosts season.

In the new season, users will be able to create AI tracks as Popits and receive boosts for them.

We are also preparing a NACKL mining scenario while listening to tracks from the bucket feed or minted Popcoins.

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
