---
hidden: true
---

# Acki Nacki News

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
