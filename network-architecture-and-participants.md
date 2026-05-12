---
description: >-
  Acki Nacki is a live network. Some protocol mechanisms are introduced
  gradually. Each capability is marked with its current status; please refer to
  it while reading the document.
---

# Network Architecture and Participants

**Acki Nacki is a multithreaded Proof-of-Stake network with parallel execution and fast probabilistic block confirmation.**&#x20;

{% hint style="info" %}
Entrypoint for interacting with the network: https://mainnet.ackinacki.org&#x20;
{% endhint %}

Its architecture separates responsibility across three main layers:

| Layer                    | Purpose                                                                                    | Main participants                                                   |
| ------------------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| Execution layer          | Distributes state across independent `threads` and processes account messages in parallel. | Block Keepers, temporary Block Producers                            |
| Consensus layer          | Produces, attests, challenges, and finalizes blocks.                                       | Block Keepers, temporary Block Producers, temporary Block Verifiers |
| Transport and user layer | Provides message delivery, network segmentation, API access, and user message ingress.     | Broadcast Proxy, Block Manager                                      |

## Capability Status

This document describes the target Acki Nacki architecture according to the white paper and the current specification.

Some mechanisms are under active development and are marked as:

| Status             | Meaning                                                                             |
| ------------------ | ----------------------------------------------------------------------------------- |
| **Available**      | Available in the current [mainnet](https://mainnet.ackinacki.org/) network.         |
| **Phased rollout** | The base mechanism works, while extensions are introduced through protocol updates. |
| **Planned**        | Described in the specification but not yet available in production.                 |

The base network functions are described separately from mechanisms that are being developed in phases.

### Status Summary

| Capability                                              | Status             | Context                                                                                                                                                                                     |
| ------------------------------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Block Keeper consensus, attestations, and finalization  | **Available**      | The base consensus layer is operational in the current network.                                                                                                                             |
| Multithreaded execution and `thread` separation         | **Available**      | Used for parallel processing of independent parts of the state.                                                                                                                             |
| Merging `threads` when load decreases                   | **Planned**        | The logic is behind a feature flag and is not enabled in the current network; merge support must be completed at the state and routing table levels.                                        |
| NACK mechanism for challenging invalid blocks           | **Phased rollout** | The challenge-and-verification flow and slashing path for the producer are available; extended accountability for invalid attestations and false accusations is being introduced in phases. |
| Extended accountability rules for invalid attestations  | **Planned**        | These rules complement the existing slashing path for producers in future protocol updates.                                                                                                 |
| Broadcast Proxy as the transport layer                  | **Available**      | Used for segmented network topology and for reducing direct load between Block Keepers.                                                                                                     |
| Block Manager and BM staking                            | **Available**      | Block Managers provide user/API access and may participate in BM staking through the corresponding license.                                                                                 |
| Mobile Verifier reward game and on-chain infrastructure | **Available**      | The on-chain reward game is active, and rewards are distributed according to the current game rules.                                                                                        |
| Light-verification flow for Mobile Verifier             | **Planned**        | Full integration of light state and transaction checks into the protocol-level security flow is being introduced in phases.                                                                 |

## Network Structure <a href="#network-structure" id="network-structure"></a>

### Threads and Execution Model <a href="#threads-and-execution-model" id="threads-and-execution-model"></a>

The Acki Nacki execution layer consists of dynamic `threads`. Each `thread` serves a subset of accounts and processes messages related to that part of the state.

This model makes it possible to:

* place related accounts in the same execution area;
* execute independent account groups in parallel;
* split overloaded `threads` as load grows;
* merge `threads` back when load decreases (**Planned**).

`Threads` do not directly share mutable state. They interact through cross-thread messages: one `thread` can pass messages to another through finalized blocks and references to them. This allows the network to execute independent parts of state in parallel and gradually synchronize account routing between participants.

### Consensus Layer

Acki Nacki consensus is maintained by a set of Block Keepers. A Block Keeper is a validator participant with active stake, signing keys, and the right to participate in block confirmation.

In each round, one Block Keeper is selected as the Block Producer for a specific `thread`. The Producer forms a new block, executes messages, adds the required metadata, signs the block, and propagates it to other Block Keepers.

The other Block Keepers verify that the block complies with protocol rules and send attestations. An attestation is a cryptographic confirmation of a block by a consensus participant.

Block confirmation happens in two stages:

| Stage           | Purpose                                                                                               |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| Prefinalization | Locks the network onto one branch and reduces the risk of fork conflicts.                             |
| Finalization    | Means the block becomes finalized under the protocol rules after the finalization conditions are met. |

### Verification and the NACK Mechanism <a href="#verification-and-the-nack-mechanism" id="verification-and-the-nack-mechanism"></a>

Acki Nacki uses lazy validation. Block Keepers are not required to fully re-execute every block on the happy path. The base security of the current `mainnet` network is provided by Block Keeper consensus, attestations, finalization rules, and the economic accountability of Block Producers for invalid blocks.

`NACK` is a challenge mechanism. If a Block Verifier detects an invalid block or an invalid state transition, it sends a signed `NACK` accusation. A `NACK` by itself does not finalize punishment and does not automatically make the accusation true: the disputed block must go through independent verification under the protocol rules.

If the accusation is confirmed:

* the invalid block and its descendants are rejected;
* the producer of the invalid block may be punished through the available slashing path (**Available**);
* the network returns to the correct branch according to the fork choice and finalization rules.

Unconfirmed or invalid accusations must not lead to slashing of honest participants. Extended accountability rules, including accountability for invalid attestations and additional rules for false accusations, are being introduced in phases (**Planned**).

{% hint style="info" %}
Thus, the current NACK flow protects the network from accepting invalid blocks through a challenge-and-verification path, while future updates extend the accountability of participants who attest to or initiate invalid actions.
{% endhint %}

### Transport Topology

The Acki Nacki transport network is not a flat mesh where every node connects to every other node. It is built from network segments.

| Segment type          | Description                                                                     |
| --------------------- | ------------------------------------------------------------------------------- |
| `Single node segment` | One Block Keeper without a Proxy, to which other participants connect directly. |
| `Proxied segment`     | One or more Proxies and a group of Block Keepers behind them.                   |

Broadcast Proxy is not a consensus participant and cannot create, modify, or attest to consensus messages on behalf of Block Keepers. Consensus messages are signed by Block Keepers, so Proxy performs only a transport function: it relays data between participants and helps reduce network load.

Proxy segments are not the only connection model. A Block Keeper may operate in a `Single node segment` without a Proxy, or use a proxy segment to optimize network propagation. For fault tolerance, participants may use several network paths and update the transport configuration when a specific Proxy is unavailable.

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

{% hint style="warning" %}
Block Producer and Block Verifier are not separate permanent node types. They are temporary functions performed by participants from the current Block Keeper set.
{% endhint %}

`Broadcast Proxy` operates separately at the transport layer: it does not participate in consensus but is an important part of the network topology.

## Block Keeper

Block Keeper is the base consensus participant in Acki Nacki.

### Main Functions

* Receive candidate blocks from Block Producers.
* Verify signatures and protocol conditions.
* Send attestations.
* Participate in prefinalization and finalization.
* Maintain fork choice and network security rules.
* Perform the Block Verifier role when necessary.

### Participation Requirements

* Have the right to participate in consensus through a [license](https://docs.ackinacki.com/for-node-owners/protocol-participation/block-keeper/license) and stake. For the first two years after `mainnet` launch, the license is required; this requirement is removed afterward.
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

* Possible for producing an invalid block, equivocation, or other violations of Block Production rules.

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

### Slashing

* The BM wallet supports a slashing mechanism.
* Slashing is tied to the state of the BM license, BM wallet, and participant lifecycle.

## Mobile Verifier

Mobile Verifier is a light participant in Acki Nacki. It is not required to run a full Block Keeper node or validate every block.

The Mobile Verifier role is related to the reward game and the development of the light-verification model. It expands user participation in network verification and creates an operational foundation for light independent checks.

### Main Functions

* Participate in the reward game for light verifiers through on-chain infrastructure;
* Perform selective checks of state or transaction activity within the light-verification flow;
* Confirm verification results according to the rules of the corresponding flow.

### Participation Requirements

* Register as a Mobile Verifier;
* Use the light-verifier client or another supported participation method;
* Follow the rules of the reward game, verification, and result confirmation.

### Rewards

* Rewards are distributed according to the reward game rules;
* Reward parameters depend on the current game rules and on-chain infrastructure.

### **Accountability:**

* Mobile Verifier is responsible for the correctness of submitted results under the rules of the corresponding flow;
* False or invalid checks are handled according to the current reward game rules and light-verification model.

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
