---
description: >-
  This page explains how Acki Nacki reaches consensus on block ordering and
  validation, including finalization and failure handling.
---

# Synchronization and Consensus Mechanism

{% hint style="info" %}
Acki Nacki assumes a Byzantine environment where strictly fewer than one third of Block Keepers are malicious.
{% endhint %}

## Model and Participants

The protocol guarantees:

* **Safety**: there are no two finalized conflicting blocks at the same height.
* **Liveness**: blocks eventually finalize, even if some participants fail or the network is slow.

Roles participating in agreement:

* **Block Producer**: deterministically selected from the active Block Keeper set for a specific round.
* **Block Keeper**: a staker with a BLS key and `SignerIndex` (`u16`). Block Keepers form the consensus voting set and attest blocks.
* **Block Verifier** (Acki-Nacki): a Block Keeper acting in the verifier role. The verifier performs targeted block re-execution when a block is challenged and creates a NACK if the block is invalid.
* **Mobile Verifier**: a lightweight participant that does not vote in consensus directly. In the current production status, the Mobile Verifier reward-game part is active; the selective light-verification flow that should reduce attack probability is under development.
* **Block Manager**: processes external messages and is not part of consensus.

The active Block Keeper set changes by Epochs through on-chain contracts. Its lifecycle is:

```
PreEpoch -> Active -> CalledToFinish -> Expired
```

Each block carries two sets:

* **Current set**: used to verify this block.
* **Descendant set**: includes the changes applied by this block and is used for the next block.

## Block Lifecycle

Synchronization in Acki Nacki is a four-phase pipeline. Each phase latches another piece of consensus state and prevents the state machine from rolling back that fact.

```
Production -> Attestation -> Prefinalization -> Finalization
```

The same active Block Keeper set can perform different functions during this lifecycle. One participant may be a producer in one round, an attester for another block, and an Acki-Nacki verifier when it checks an accused block. Consensus rewards are distributed within the Block Keeper group and do not depend on which of these functions a participant happened to perform in a specific Epoch.

### Production

The producer for a round is determined **deterministically** from the parent's `ProducerSelector` and the round number. No coordination is needed: all nodes independently know who the producer is.

Happy-path flow:

1. The producer executes the message queue through TVM on top of the parent state.
2. It wraps the TVM block with Acki Nacki metadata: thread, round, keeper set changes, and cross-thread references.
3. It signs the block with BLS and broadcasts it.
4. The other Block Keepers accept the block, verify signatures, and start attesting.

A block may include external messages from the API, internal messages from the thread queue in round-robin order, NACK slashing messages, and keeper set changes.

### Block Verifier (Acki-Nacki)

The Acki-Nacki verifier is the correctness-checking function inside the Block Keeper group. It is separate from the producer function: the producer creates and signs the block, while the verifier can independently reconstruct the block from the parent state and the same input queues.

Verifier responsibilities:

* verify the block against the parent state when revalidation is required;
* detect invalid state transitions, excessive execution, or mismatched observable outputs;
* create and sign a NACK when the block is invalid;
* provide evidence that can invalidate the accused block and its descendants;
* expose wrong accusations to a possible `WrongNack` counter-claim.

This verifier path is intentionally lazy. Acki-Nacki does not require every Block Keeper to re-execute every block before attesting. Instead, the verifier role provides an accountability path: a bad block can be challenged, re-executed independently, and either rejected or confirmed despite the accusation.

### Attestation

Each Block Keeper signs `AttestationData` with its BLS key. Individual signatures are **aggregated** into one compact BLS signature, which keeps the protocol scalable with large validator sets.

| Path         | Threshold                | Deadline        | When It Triggers               |
| ------------ | ------------------------ | --------------- | ------------------------------ |
| **Primary**  | `ceil(2N/3)` (`~66%+`)   | `beta` blocks   | Healthy network, fast finality |
| **Fallback** | `floor(N/2)+1` (`50%+1`) | `2*beta` blocks | Primary did not collect 66%    |

{% hint style="info" %}
Primary optimizes for fast finality. Fallback preserves liveness when messages are delayed but enough participants remain online.
{% endhint %}

Votes are staggered with:

```
delay = pulse_timeout * (0.8 + distance_from_producer)
```

Block Keepers closest to the producer vote first. This gives the block time to propagate and makes signature aggregation more orderly.

Aggregation first tries an optimistic merge: merge all signatures at once and verify once. If that fails, the protocol falls back to one-by-one merging and marks invalid signatures as `poisoned`; these signatures become evidence for slashing.

### Prefinalization

**Prefinalization** happens when a block receives the first `50%+1` attestations. Any two `50%+1` sets must intersect. If all Block Keepers are honest, this gives a single prefinalized branch. If a conflict appears, the intersection becomes cryptographic proof of double-signing.

Once a block is prefinalized:

* Block Keepers lock onto it through ActionLock.
* Competing blocks at the same height and their descendants are invalidated.
* The network converges on one chain.

Sibling resolution handles two prefinalized siblings with the same parent:

* **Higher round wins**: the higher-round block was produced later, after more time for convergence; the lower-round block is invalidated.
* **Same round**: the producer double-signed, and both blocks become cryptographic proof for stake slashing.

Prefinalization is write-once: once the signal is set, it cannot be unset. This prevents the state machine from oscillating.

### Finalization

Prefinalization protects against forks, but it does not guarantee irreversibility. A prefinalized block can still be NACKed as invalid. Finalization is the actual commit.

A block becomes final when all eight conditions are satisfied:

1. The parent is finalized.
2. Signatures are verified.
3. The block is not invalidated.
4. All NACK accusations are resolved.
5. The block is applied to the parent state and TVM execution succeeds.
6. All cross-thread references are finalized.
7. Cross-thread reference data is available and prepared.
8. The block appears in `finalizes_blocks` of the corresponding checkpoint-carrier block.

Attestation checkpoints avoid direct vote counting. Each new block carries checkpoints for its ancestors, and the checkpoint distance grows as the chain advances. When the deadline is reached, the vote set is checked.

Primary finalization requires `ceil(2N/3)` attestations by the `beta` deadline. Fallback finalization is two-stage:

1. At `beta`, check for `50%+1` attestations with Primary target type.
2. At `2*beta`, check for `50%+1` attestations with Fallback target type.

Both fallback stages must pass. If they fail, the checkpoint fails, the block does not finalize, and it should be NACKed.

After finalization:

* the finalized block is marked and persisted;
* the attestation cutoff advances;
* descendants are notified through `has_parent_finalized`;
* raw block data is archived;
* all non-finalized blocks at this height or lower are invalidated.

{% hint style="warning" %}
Invalidation is recursive and thread-agnostic. Invalidating a block cascades to all descendants in all threads. A finalized block cannot be invalidated.
{% endhint %}

## ActionLock

ActionLock is the root of double-sign protection. Before sending an attestation or a `NextRound` request, a Block Keeper updates ActionLock and saves it in durable storage.

ActionLock records:

* parent block;
* current round;
* the block already voted for at this height, if any;
* known NACKed blocks at this height.

In production, ActionLock is stored through a durable storage backend. This does not necessarily mean a literal `fsync` of a local file. Some authority-switch states related to starting a new producer still have durability TODOs in the code, so ActionLock should not be described as the only persisted guard for every possible production action.

Lock rules:

* One vote per `(height, round)`.
* A block with a **higher round** may overwrite the lock because a higher round means later network convergence.
* A block with the **same round** but a different hash is rejected for attestation, because signing it would be double-signing.

{% hint style="danger" %}
The durability requirement is protection for the Block Keeper itself. A vote must be persisted before it is sent so that a crash cannot cause accidental self-slashing.
{% endhint %}

## Authority Switch

If the round producer is silent or works invalidly, the network switches authority to the next producer without waiting forever.

### Round Buckets

Round duration grows by buckets. This gives the network a fast response to temporary failure without burning through the producer queue.

| Bucket  | Approximate Round Duration | Rotations |                                               Purpose |
| ------- | -------------------------: | --------: | ----------------------------------------------------: |
| Fast    |                     `~1 s` |      `60` | First response, about 10 minutes for 10 Block Keepers |
| Medium  |                    `~60 s` |       `1` |                    After the fast bucket is exhausted |
| Default |                   `~5 min` | unlimited |                           Final pace for long outages |

### Round-Switch Protocol

1. **Detection**: nodes see that there is no valid block in the current round.
2. **Request**: each node sends `NextRound` to the next producer with its `Lock`.
3. **Quorum**: the next producer waits for a strict majority, `floor(N/2)+1`. Requests with a different NACK set are filtered out.
4. **Resolution**: the block with the highest round is selected from the collected locks.
5. **Rejection**: if the new producer already has a prefinalized block at this height, it replies with `NextRoundReject` and proof of prefinalization.

If a highest-round locked block exists, the producer validates it, aggregates attestations, and broadcasts `NextRoundSuccess`. If no locked block exists, the producer builds a new one if at least `330 ms` remain; otherwise it waits for the next round.

`NextRoundSuccess` messages that arrive before a node is ready are buffered and replayed when the node reaches the required height.

## Node State Synchronization

Consensus synchronization decides which blocks are accepted and finalized. Node state synchronization is the recovery path that lets a joining or lagging node reach that finalized history without replaying the entire network from genesis.

A node enters synchronization when:

* it starts without a usable finalized block for the thread;
* the gap between an incoming block and the local applied/finalized chain exceeds `need_synchronization_block_diff`;
* authority-switch processing detects that the node cannot safely continue from its local view.

### Joining Flow

When a node needs state, it broadcasts `NodeJoining` or `NodeJoiningWithLastFinalized` with its last finalized position. Peers compare that position with their own finalized chain and answer in one of two ways:

* by sending candidate/finalized blocks that can bridge a small gap;
* by sending `SyncFinalized` or `SyncFinalizedWithHeight`, which points to a finalized block and the shared state resources for the relevant thread references.

If the gap is small enough, the node requests the missing block range through `BlockRequest`. Responses include the requested blocks in range, with finalized blocks first and then candidates. If the gap is too large, or a block range cannot be served, the serving side can schedule a future block with `share_state_resources`. After that block finalizes, state is published for synchronization.

### Snapshot Loading

State snapshots are anchored to a finalized block by sequence number or block height. A shared state package contains the data a node needs to resume safely from that finalized point:

* the Block Keeper set;
* the existing thread list;
* the state for each thread;
* cross-thread reference data needed to continue message delivery;
* enough finalized block context to restart processing from the snapshot anchor.

The node downloads the snapshot through the state-sync service, validates that the snapshot still matches the active synchronization target, and applies it only if it is newer than the local finalized state or the local chain can bridge the difference. While a snapshot is loading, the node continues broadcasting `NodeJoining`; newer `SyncFinalized` candidates can replace stale targets so the node converges on the best available finalized state.

After a snapshot is applied, the node resumes normal block processing from the anchor. Incoming candidate blocks are then handled by the regular consensus pipeline: signature checks, attestation, prefinalization, finalization, NACK handling, and cross-thread reference preparation.

## NACK

NACK is the lazy validation and punishment path.

{% hint style="info" %}
Blocks are not re-executed by all Block Keepers on receipt. Block Keepers can attest after signature and consensus checks. Full TVM re-execution is performed only along selected validation paths and when a NACK accusation must be resolved, with the NACK being created by the Block Verifier (Acki-Nacki).
{% endhint %}

NACK flow:

```
Acki-Nacki verifier re-executes a block -> finds it invalid -> NACK with reason
   v
BLS signature and broadcast
   v
Other BKs record the accusation
   v
Block is sent to validation service for independent revalidation
   v
Confirmed invalid -> block and descendants invalidated + slashing message
Confirmed valid   -> possible counter-NACK (WrongNack) against accuser
```

A block cannot finalize while it has unresolved NACKs. Resolution means either there are no accusations or `validated == true` after independent checking.

During revalidation, the Acki-Nacki verifier takes the parent state, re-executes all messages, and assembles an independent block:

1. Exact match means the block is valid.
2. Difference triggers partial comparison of observable outputs: metadata, value flow, message queue changes, and final state hash. Differences in internal execution paths may be tolerated when outputs are deterministic.

## Cross-Thread Synchronization

Acki Nacki is multithreaded: each thread has its own chain. A block can reference **finalized** blocks from other threads to import cross-thread messages.

References are selected from blocks whose cross-thread data is available and finalized. When `delay-references` is enabled, references are delayed by `sqrt(thread_count)` generations so that all threads do not point to the newest block at the same time and create contention.

## Economic Synchronization Layer

Protocol guarantees are multiplied by economics:

* **Reputation coefficient `R in [1, 3]`**: a reward multiplier for continuous participation. It grows exponentially and saturates after roughly five years. Missing an Epoch resets it to `R_min`.
* **Mobile Verifiers**: in the target security model, they multiply attack complexity. The classic probability `(1 - v/(N-1))^(A - N_mal)` receives an additional factor `(1 - lambda_MV)^(N_MV - N_MV_mal)`. In the current production contour, this security contribution is not active yet.
* **Stake with cooling period**: stake is split into an active half and a cooling half used for slashing calculation.
* **No protocol-level delegation**: the Epoch contract accepts only messages signed by the Block Keeper's own private key, not by a delegate.
* **Fair rewards**: within the Block Keeper group, reward does not depend on whether the participant was a producer, Acki-Nacki, or Block Keeper in a specific Epoch.

## End-to-End Scenarios

### Happy Path

```
Round R: producer P builds block B on the parent
  -> P signs B and broadcasts it
  -> BKs attest, staggered by distance from P
  -> 50%+1 attestations: B is prefinalized
  -> at least two thirds within beta: B finalizes through primary path
  -> finalization loop commits B and the chain advances
```

### Slow Network

```
Round R: producer P builds B
  -> BKs attest slowly
  -> primary deadline beta passes without 66%
  -> B transitions to fallback path
  -> at 2*beta: if 50%+1 fallback attestations exist, B finalizes
  -> otherwise, B fails and is NACKed
```

### Producer Failure

```
Round R: producer P is offline, no block
  -> BKs detect stall by round timing
  -> send NextRound to the next producer Q
  -> Q collects majority floor(N/2)+1
  -> if someone locked a block: Q broadcasts it as the winner
  -> if nobody did: Q builds a new block if there is time
  -> authority moves to Q
```

### Invalid Block

```
Round R: producer P produced invalid B
  -> some BKs attested without re-execution
  -> an Acki-Nacki verifier re-executes and finds it invalid
  -> the verifier broadcasts NACK
  -> other BKs record the accusation
  -> validation service independently revalidates B
  -> B and all descendants are invalidated
  -> slashing message is included in the next valid block
  -> Authority Switch moves to the next producer
```

### Double-Sign

```
Round R: producer P signed both B1 and B2
  -> both collected attestations
  -> both reached 50%+1 and are prefinalized
  -> sibling resolution detects the same round
  -> protocol violation: slashing
  -> both blocks are cryptographic proof of P's equivocation
```

## Safety, Liveness, and Accountability

**Safety**:

* Prefinalization at `50%+1` implies intersection of quorum sets.
* Honest Block Keepers do not sign conflicting blocks thanks to ActionLock.
* If conflicting proofs still appear, the intersection gives double-sign evidence.
* Finalization requires prefinalization and all validity conditions, so conflicting blocks cannot both become final.

**Liveness**:

* Healthy producer and network within deadline leads to primary finalization.
* Producer stalls are handled by round buckets and authority switch.
* Slow attestations are handled by fallback finalization.
* Round advancement requires only `floor(N/2)+1`, not 66%.
* Lagging nodes recover through block-range requests or finalized state snapshots.

**Accountability**:

* Double-signing is detected through conflicting prefinalization proofs.
* Invalid blocks are detected through lazy revalidation.
* ActionLock durability prevents a Block Keeper from denying what it signed.
* Slashing messages are included in blocks and become part of chain history.

## Key Synchronization Latches

| Layer            | Mechanism                                                   | What It Fixes                       |
| ---------------- | ----------------------------------------------------------- | ----------------------------------- |
| Local node       | **ActionLock** with durable persistence                     | One vote in `(height, round)`       |
| Network, phase 1 | **Prefinalization @ 50%+1**                                 | Branch lock or evidence on conflict |
| Network, phase 2 | **Finalization @ `ceil(2N/3)` / 50%+1 + 8 conditions**      | Irreversibility                     |
| Recovery         | **Authority Switch @ `floor(N/2)+1`** and round buckets     | Progress under failure              |
| Node sync        | **NodeJoining + BlockRequest + SyncFinalized snapshots**    | Catch-up from finalized state       |
| Correctness      | **NACK + lazy revalidation**                                | Filtering invalid blocks            |
| Cross-thread     | **References only to finalized blocks** and `sqrt(N)` delay | Consistency between threads         |
| Economics        | **Stake + Reputation + Mobile Verifiers**                   | Incentives to follow the protocol   |
