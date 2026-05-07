---
description: >-
  Acki Nacki is a live network. Some protocol mechanisms are introduced
  gradually. Each capability is marked with its current status; please refer to
  it while reading the document.
---

# Network Architecture and Participants

**Acki Nacki is a multithreaded Proof-of-Stake network with parallel execution and fast probabilistic block confirmation.** \
Its architecture separates responsibility across three main layers:

| Layer                    | Purpose                                                                                    | Main participants                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| Execution layer          | Distributes state across independent `threads` and processes account messages in parallel. | Block Keepers, temporary Block Producers                            |
| Consensus layer          | Produces, attests, challenges, and finalizes blocks.                                       | Block Keepers, temporary Block Producers, temporary Block Verifiers |
| Transport and user layer | Provides message delivery, network segmentation, API access, and user message ingress.     | Broadcast Proxy, Block Manager                                      |

## Capability Status

This page describes the target Acki Nacki architecture according to the white paper and current specification. Core network functions are separated from mechanisms that are still being rolled out.

| Status             | Meaning                                                                             |
| ------------------ | ----------------------------------------------------------------------------------- |
| **Available**      | Available in the current network.                                                   |
| **Phased rollout** | The base mechanism works, while extensions are introduced through protocol updates. |
| **Planned**        | Described in the specification but not yet available in production.                 |

### Status Summary

| Capability                                              | Status             | Context                                                                                                                                              |
| ------------------------------------------------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Block Keeper consensus, attestations, and finalization  | **Available**      | The base consensus layer is running in the current network.                                                                                          |
| Multithreaded execution and `thread` separation         | **Available**      | Used for parallel processing of independent parts of the state.                                                                                      |
| Merging `threads` when load decreases                   | **Planned**        | The logic is behind a feature flag and is not enabled in the current network. Merge support at the state and routing table levels must be completed. |
| NACK mechanism for challenging invalid blocks           | **Phased rollout** | The base challenge flow and producer slashing path are available. Additional protocol guarantees are introduced through updates.                     |
| Extended accountability rules for invalid attestations  | **Planned**        | These rules will be added to the existing producer slashing mechanism in future protocol updates.                                                    |
| Broadcast Proxy as the transport layer                  | **Available**      | Used for segmented network topology and for reducing direct load between Block Keepers.                                                              |
| Block Manager and BM staking                            | **Available**      | Block Managers provide user and API access and can participate in BM staking through the corresponding license.                                      |
| Mobile Verifier reward game and on-chain infrastructure | **Available**      | The on-chain reward game is active, and rewards are distributed according to the current game rules.                                                 |
| Light-verification flow for Mobile Verifier             | **Planned**        | Full integration of light state and transaction checks into the protocol-level security flow is introduced gradually.                                |

## Network Architecture

### Execution Layer: Threads

The Acki Nacki execution layer consists of dynamic `threads`. Each `thread` serves a subset of accounts and processes messages related to that part of the state.

This model allows the network to:

* place related accounts in the same execution area;
* execute independent account groups in parallel;
* split overloaded `threads` as load grows;
* merge `threads` back when load decreases (**Planned**).

`Threads` do not directly share mutable state. They interact through cross-thread messages: one `thread` can pass messages to another through finalized blocks and references to them. This lets the network execute independent parts of the state in parallel and gradually synchronize account routing among participants.

### Consensus Layer

Acki Nacki consensus is maintained by Block Keepers. A Block Keeper is a validator participant with active stake, signing keys, and the right to participate in block confirmation.

In each round, one Block Keeper is selected as the Block Producer for a specific `thread`. The Producer creates a new block, executes messages, adds the required metadata, signs the block, and distributes it to other Block Keepers.

Other Block Keepers check the block against protocol rules and send attestations. An attestation is a cryptographic confirmation of a block by a consensus participant.

Block confirmation has two stages:

| Stage           | Purpose                                                                                              |
| --------------- | ---------------------------------------------------------------------------------------------------- |
| Prefinalization | Locks the network onto one branch and reduces the risk of fork conflicts.                            |
| Finalization    | Makes the block an irreversible part of network history after all security conditions have been met. |

### Verification and NACK

Acki Nacki uses lazy validation. Under normal conditions, each Block Keeper does not have to fully re-execute every received block. Instead, the network uses a challenge mechanism.

If a Block Verifier detects an invalid block or an incorrect state transition, it sends a signed `NACK` accusation. The disputed block then goes through independent verification.

If the accusation is confirmed:

* the invalid block and its descendants are rejected;
* the malicious producer can be punished through the available slashing path;
* additional accountability rules may apply to participants who attested to the invalid block (**Planned**).

{% hint style="success" %}
The NACK mechanism keeps block processing fast on the happy path while preserving an economic punishment mechanism for serious protocol violations.
{% endhint %}

### Transport Topology

The Acki Nacki transport network is not a flat mesh where every node connects to every other node. It is built from network segments.

| Segment type          | Description                                                                     |
| --------------------- | ------------------------------------------------------------------------------- |
| `Single node segment` | One Block Keeper without a Proxy, to which other participants connect directly. |
| `Proxied segment`     | One or more Proxies and a group of Block Keepers behind them.                   |

Broadcast Proxy is not a consensus participant. Its purpose is to reduce network load, improve message propagation, and help scale the transport layer.

### User Layer

Block Managers sit outside the consensus core. They subscribe to Block Keeper streams, maintain a local blockchain data database, provide APIs, and receive external messages from users or applications.

The Block Manager forwards received external messages to the current Block Producers of the corresponding `threads`.

In the current model, a Block Manager can participate in BM staking if it has the corresponding license. Rewards are accrued through the BM wallet and depend on BM staking parameters and network state.

## Network Participants

The public network model includes the following main roles:

* `Block Keeper`;
* `Block Producer`;
* `Block Verifier / Acki-Nacki`;
* `Block Manager`;
* `Mobile Verifier`.

Block Producer and Block Verifier are not separate permanent node types. They are temporary functions performed by participants from the current Block Keeper set.

`Broadcast Proxy` operates separately at the transport layer: it does not participate in consensus but is an important part of the network topology.

{% hint style="warning" %}
Block Producer and Block Verifier are not separate permanent node types. They are temporary functions performed by participants from the current Block Keeper set.
{% endhint %}

## Block Keeper

Block Keeper is the base consensus participant in Acki Nacki.

### Main Functions

* Receive candidate blocks from Block Producers.
* Verify signatures and protocol conditions.
* Send attestations.
* Participate in prefinalization and finalization.
* Maintain fork choice and network security rules.
* Perform the Block Verifier role when needed.

### Participation Requirements

* Have the right to participate in consensus through a license and stake. For the first two years after mainnet launch, the license is required; this requirement is removed afterward.
* Have signing keys for consensus participation.
* Run and maintain an available Block Keeper node.
* Be in the active consensus participant set.

### Rewards

* Receives rewards for participating in consensus and maintaining network security.
* Rewards are tied to Block Keeper participation in the network, not to separate temporary roles.

### Slashing

* Slashing mechanics are available for serious protocol violations, including producing invalid blocks. Specific violation detectors and rules apply according to the current protocol version (**Available**).
* Additional accountability rules for invalid attestations will be introduced in protocol updates (**Planned**).

## Block Producer

Block Producer is a Block Keeper selected to produce a block in a specific round and `thread`.

### Main Functions

* Select and process messages.
* Execute transactions.
* Create a block.
* Include state updates, metadata, and cross-thread references.
* Sign and distribute the candidate block.

### Participation Requirements

* Be an active Block Keeper.
* Be selected as the producer for a specific round and `thread`.
* Have up-to-date state and network availability.

### Rewards

* The separate temporary Block Producer role does not have an independent reward.
* Rewards are received through participation of the corresponding Block Keeper in consensus.

### Slashing

* Possible for producing an invalid block, equivocation, or other violations of block production rules.

## Block Verifier / Acki-Nacki

Block Verifier, or Acki-Nacki, is a Block Keeper that verifies a disputed block.

### Main Functions

* Detect invalid blocks and incorrect state transitions.
* Send a `NACK` when a violation is detected.
* Participate in independent verification of the disputed block.
* Help the network reject invalid branches after a confirmed `NACK`.

### Participation Requirements

* Be an active Block Keeper.
* Be able to verify the block and sign the verification result.
* Have access to the required state for verification.

### Rewards

* The public model does not define a separate permanent verifier reward.
* The role protects network security and the stake of honest participants.

### Slashing

* A confirmed `NACK` can lead to punishment of the invalid block's producer through the available slashing path (**Available**).
* Accountability for false or incorrect accusations is introduced as part of the accountability layer development (**Planned**).
* Additional rules for invalid attestations will be introduced in protocol updates (**Planned**).

## Broadcast Proxy

Broadcast Proxy is a transport participant in the network. It is not part of the consensus set and does not make consensus decisions.

### Main Functions

* Relay protocol messages between network segments.
* Reduce direct network load between Block Keepers.
* Improve scaling of block and message propagation.

### Participation Requirements

* Have a valid network configuration.
* Be available to participants that use the corresponding proxy segment.
* Maintain stable operation of the transport layer.

### Rewards

* Proxy is not a consensus reward role. Economic terms may be defined operationally between infrastructure participants.

### Slashing

* Proxy does not have consensus stake and is not slashable as a Block Keeper.
* An incorrectly operating Proxy may be removed from the configuration or replaced by the operator.

## Block Manager

Block Manager is a participant in the user-facing and infrastructure part of the network. It is not part of the consensus set, but it provides users and applications with access to the blockchain.

### Main Functions

* Subscribe to Block Keeper streams.
* Store a local blockchain data database.
* Provide APIs and blockchain data services.
* Receive external messages.
* Route external messages to the current Block Producers.
* Maintain fault-tolerant access to multiple Block Keepers.

### Participation Requirements

* Have a BM wallet and delegated BM license.
* Run the Block Manager service.
* Have access to Block Keeper streams and API endpoints.
* Maintain local blockchain data storage.

### Rewards

* Participates in BM staking through the corresponding license.
* Rewards are accrued through the BM wallet according to BM staking parameters and network state.
* The current implementation uses a simplified reward model that differs from the formula in the White Paper.

### Slashing

* The BM wallet supports a slashing mechanism.
* Slashing is tied to the state of the BM license, BM wallet, and participant lifecycle.

## Mobile Verifier

Mobile Verifier is a lightweight Acki Nacki security participant. It does not have to run a full Block Keeper node or validate every block.

### Main Functions

* Participate in the reward game for lightweight verifiers through on-chain infrastructure (**Available**).
* Perform independent checks of selected parts of the state or transaction activity through the light-verification flow (**Planned**).

### Participation Requirements

* Register as a Mobile Verifier and participate in the reward game (**Available**).
* Use the light-verifier client as it becomes publicly available (**Planned**).
* Follow the rules for verification and result confirmation.

### Rewards

* The reward game for Mobile Verifiers is active, and rewards are distributed according to the current game rules (**Available**).
* Integration of light state checks with the reward flow is introduced gradually (**Planned**).

### Slashing

* The accountability model for Mobile Verifier evolves together with the light-verification flow (**Planned**).
* Rules for false or incorrect checks will be published as part of future updates (**Planned**).

## Operational and Economic Entities

In addition to protocol roles, operational and economic entities exist around the network:

* `License Owner` - the owner of a license associated with participation or delegation rights;
* `Node Owner / Node Provider` - an infrastructure operator that runs and maintains nodes;
* `Proxy Provider` - an operator of proxy infrastructure for network optimization.

These entities are important for deployment, licensing, delegation, and network operations, but they should not be considered independent consensus participants. Consensus responsibility remains with Block Keepers and temporary roles inside the consensus set.

## Summary

Acki Nacki can be understood as a multilayer system:

* `threads` provide parallel execution and account routing;
* Block Keepers maintain consensus, attestations, and finality;
* Block Producers temporarily create blocks inside `threads`;
* Block Verifiers challenge invalid blocks through NACKs;
* Broadcast Proxy scales the transport layer between segments;
* Block Managers provide user access and external message ingress;
* Mobile Verifiers participate in the reward game today and expand the security surface with lightweight independent checks as the light-verification flow develops.

In short, Acki Nacki is a BK consensus network with parallel execution through `threads`, segmented transport topology, user access through Block Managers, and additional verification through Mobile Verifiers that is being developed gradually.
