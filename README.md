# Acki Nacki Overview

{% hint style="info" %}
**For developer documentation, please visit** [**the developer portal**](https://dev.ackinacki.com/)
{% endhint %}

## **TLDR** <a href="#secured-decentralized-fast" id="secured-decentralized-fast"></a>

Acki Nacki blockchain technology is a Rust implementation of a novel consensus protocol: ["Acki Nacki: A Probabilistic Proof-of-Stake Consensus Protocol with Fast Finality and Parallelisation"](https://link.springer.com/chapter/10.1007/978-3-031-61486-6_4) and ["Acki Nacki Binary System"](https://tokenomics.ackinacki.com/tokenomics.pdf) tokenomics research.&#x20;

Acki Nacki is asynchronous, highly effective proof-of-stake protocol optimized for fast finality, while allowing for high throughputs via execution parallelization.&#x20;

It is a probabilistic protocol that achieves higher Byzantine fault tolerance than Nakamoto, BFT (including Hotstuff and AptosBFT), Solana, and other modern consensus protocols.&#x20;

Our protocol reaches consensus in two communication steps and has a total number of messages that are subquadratic to the number of nodes, with probabilistic, dynamically adjusted safety guarantees.&#x20;

We trade off deterministic consensus with theoretical constraints on message complexity and the number of Byzantine agreements, with probabilistic algorithms overtaking these boundaries. We further claim that because of the use of randomness and socioeconomics in blockchain designs, no real trade-off is actually present.

One of the key ingredients of our approach is separating the verification of execution by a consensus committee from the attestation of block propagation by network participants.&#x20;

Our consensus committee is randomly selected for each block and is not predetermined, while the Leader is deterministic.

## Introduction

Current public blockchains are almost exclusively used for financial applications, be it for the store and transfer of value or decentralized finance. Users are ready to pay gas and transaction fees when transacting in value. The primary reason for this user experience inefficiency is the inherent lack of performance in both transaction execution throughput and time to finality, due to strict requirements on state validation.

We present a highly efficient, scalable, and practical blockchain protocol optimized for heavy parallelization and extremely fast finality times. The goal of the protocol is to produce performance comparable to cluster cloud databases without compromising security.

## **Background**

Consensus protocols in computer science are categorized into probabilistic and deterministic. Since 1978, deterministic protocols have evolved, leading to pBFT's creation for varied applications without addressing decentralized money's double spending challenge.

#### **Bitcoin**

Nakamoto's Bitcoin, introduced on October 31, 2008, first solved double spending with a Proof-of-Work (PoW) probabilistic consensus. Miners compete by solving cryptographic puzzles for block proposal rights, embedding economic incentives into its security. Despite its pioneering role, Bitcoin's low transaction rate (7 transactions per second) and long finalization times limit its functionality beyond a store and transfer of value.

#### **BFT**

Before Ethereum in 2014, S. King and S. Nadal proposed Proof-of-Stake (PoS) in 2012, enabling deterministic protocols like pBFT in cryptocurrencies by staking assets. This shift allowed enhancements over pBFT, but BFT's deterministic nature is diluted in PoS due to economic variables and probabilistic encryption, aligning its security more with cryptography and game theory despite performance sacrifices.

#### **Fast Byzantine Paxos**

This protocol, aiming for rapid asynchronous consensus, requires consensus in two steps normally but necessitates >= 5 \* f + 1 nodes to manage f Byzantine nodes, a stricter requirement than pBFT. It illustrates the deterministic protocols' limitations in efficiently addressing malicious nodes.

### **Modern Blockchains**

Recognizing performance problems of Nakamoto and BFT consensus protocols lately few other approaches surfaced. We will compare with three most performant of them: Solana, Avalanche, Aptos, Algorand and Dfinity.

#### **Solana**

Solana stands out as a blockchain platform designed for decentralized applications (dApps), focusing on high scalability and efficiency. It boasts a superior transaction processing capability, enabling higher transactions per second (TPS) with lower fees. Solana combines Proof of Stake (PoS) with Proof of History (PoH), a concept introduced by Yakovenko, to verify time passage between events, enhancing consensus efficiency. This approach uses validator clusters instead of individual nodes, though its scientific basis has faced scrutiny.

#### **Avalanche**

The Avalanche consensus mechanism involves nodes conducting repeated voting among a small group of validator nodes to determine transaction acceptance. Validators respond with their preferred transaction, and if a significant majority agrees, that transaction is chosen. Parameters like the threshold for agreement and Confidence Threshold are adjustable. Avalanche subsampling has low message overhead regardless of the number of validators. Transitive voting helps with transaction throughput. Delays can occur when multiple blocks are proposed simultaneously. The attack probability in Avalanche dynamically changes based on input parameters, affecting finalization time and message complexity.

#### **AptosBFT**

The Aptos builds on advanced pBFT variants like Hotstuff, focusing on random leader selection for each block to improve consensus. However, this necessitates frequent message replication across all network nodes, adding quadratic complexity and impacting overall performance.

#### **Algorand**

The Algorand is a strongly synchronous 3-step protocol. Algorand relies on a public randomness computed in previous blocks. It is used to elect a committee of leaders and a committee of block validators at each round that will have a sufficient number of honest nodes with overwhelming probability. The Algorand’s communication steps consist of: gossip propagation of blocks from all network leaders, BFT among the committee of validators, and the final gossip propagation of the block to the network. The Algorand requires more than 2/3 of the nodes in the validator committee to be honest to ensure security guarantees during the execution of the BFT protocol.

#### **Dfinity**

The Dfinity consensus protocol has four layers: client identities, decentralized random beacon, blockchain with leader ranking, and decentralized notary for finality. Security assumption requires a certain ratio of honest to malicious nodes. The consensus process involves 4 steps with different latencies based on adversary types. In the original paper, the message complexity is unbounded. However, the proposed modification reduces the expected communication complexities against an adaptive adversary, and against a static or mildly adaptive adversary

## **Sharding**

The Sharding a method aimed at boosting performance through data and execution partitioning, was pioneered by Zilliqa and further developed in Ethereum for state sharding. Protocols like TON, Near, and Elrond also employ sharding with parallel leader selection and state synchronization to enhance throughput. Despite these advancements, sharding and parallel execution technologies have yet to surpass a practical limit of around 100K TPS in controlled tests, indicating persistent scalability challenges in blockchain consensus mechanisms.

## **Construction of Acki Nacki**

The Acki Nacki probabilistic consensus protocol intends to take the performance of fault-tolerant consensus protocols as far as we can. In Acki Nacki, Validator Node can perform three roles: Block Producer, Block Keeper and Verifier (which we call the Acki-Nacki entity). All of these roles could be performed by any network participant in parallel. So many Acki Nacki chains (called Threads) can exist simultaneously, but since their security and functionality do not depend on each other.

### **Definitions** <a href="#definitions" id="definitions"></a>

**Account (contract)** is a record in a distributed database with balance, code and data.

**Thread** is a subset of nodes that serve a particular subset of Accounts.

**Block** is an object that contains new transactions which, once finalized, are stored in the blockchain as a permanent and immutable part of this decentralized ledger. Each block includes information such as transaction data, timestamp, common section, and a reference to the previous block, creating a chain of interconnected blocks.

**Common section** is the section of the block that contains information that needs to be shared among all network participants.

**Validator Node** is a Acki Nacki network participant that can be in one of the three modes : Block Producer, Block Keeper or Acki Nacki at a certain moment.

**Block Producer (BP)** is a leader of a particular Thread responsible for block production.

**Block Keeper (BK)** is an entity having two functions: Receives blocks from BP and sends out an Attestation with block hash and other metadata back to BP. BK does not check block transactions validity, it does not try to execute the block, only apply it to its local state with a mark 'Not Final'. Performs a self check if it needs to become a Verifier for this block as described below. If it does, BK will verify the Block and broadcast the result: Ack, if the Block is ok and Nack, if the block is invalid.

**Verifier (Acki-Nacki)** — is a BK being responsible for block validation and notifying all network participants about his verdict: is block valid or not.

**Attestation** — message that is sent to BP by any BK after receiving the block. Attestation is BLS signature done on BK’s private key. BP of the next block must aggregate all received Attestations for the previous block into one BLS signature and include it into the Common section of the new block.

**Ack** — Verifier’s message that is broadcasted to all network participants by Acki-Nacki if the block is verified and it’s valid.

**Nack** — Verifier’s message that is broadcasted to all network participants by Acki-Nacki if the block is verified and it’s NOT valid.

_Attestations and Verifier’s messages must contain block hash, its BLS signature on BK’s private key. Some extra data may be added. For example, Nack contains the reason for block rejection._

### **Security Assumptions**

We follow standard assumptions of Safety and Liveness properties for Acki Nacki protocol. These properties make the network operation look like the operation of a monolithic valid server i.e. linearizable consistent block ledger. Safety: there are no two honest BK accepting different blocks of the same height and no block with an incorrect transaction is being finalized Liveness: If an honest BP has received a transaction then it will be eventually included in every honest node’s ledger.

In accordance with these properties, we classify the attacks violating them:

**Safety attacks**

Such attacks include dissemblance and private chain attacks. Dissemblance means that the adversary maintains Byzantine nodes to send different messages to different nodes, which may lead to nodes’ disagreement. Private chain attacks mean that the adversary controls Byzantine nodes to work on a separate blockchain privately while following the protocol.

**Liveness attacks**

Liveness attacks. These types of attacks include the aforementioned dissemblance and withholding attacks. Apart from safety, dissemblance may retain honest nodes to make decisions forever, breaking the liveness. Retaining means that the adversary which controls Byzantine nodes doesn't send messages to particular nodes, which may also retain them to make decisions forever.

### **Block Producer selection**

In Acki-Nacki, the selection of Block Producers (BPs) is not random due to security concerns regarding potentially malicious BPs. Instead, a deterministic algorithm is used where the hash of a block with a shard split or other message serves as a seed for random sampling of one BP from a sorted list of BPs' public keys. The current list of BPs is always included in the Common Section of any Block, which also contains other block-related data such as Attestations, Verifier's messages, slashing/reward conditions, etc.

### **Acki-Nacki Selection Algorithm**

The Acki-Nacki selection algorithm in the Acki Nacki blockchain involves calculating a value based on the block and the BP's secret key, then determining if it meets certain criteria to be considered Acki-Nacki. The process ensures randomness in selecting Acki-Nacki BPs, with each selection being an independent event. This method allows for control over the average number of Acki-Nacki per block.

### **Block Production and broadcast**

In the new block production and broadcast process, a Block Producer (BP) releases a new block every 330 milliseconds by collecting unprocessed messages, executing transactions, and creating a block within certain limits. The BP signs the block with its BLS private key and broadcasts it to all Block Keepers (BKs). Upon receiving the block, a BK computes an Attestation for the block, and sends it back to the BP.

### **Block Verification**

Block verification in the protocol is conducted by Acki-Nacki entities selected through an algorithm. These entities must validate blocks and send Ack/Nack messages to the network to avoid being slashed. Third parties can also validate blocks with a bond but are not obligated to do so. If a Block Producer (BP) creates a block with complex execution, Acki-Nacki may delay verification which may lead to incorrect transactions being accepted. To counter this safety attack, Verifiers will stop after 330 ms and send a special Nack with a "too complex" message, allowing the committee to penalize the BP if necessary.

### **Acki-Nacki Selection Proof**

In the Acki-Nacki selection proof process, a Block Keeper (BK) generates BLS key pairs sorted by sequential number for block verification. The BK commits the hash of each key pair to the network using a Merkle Tree. After each block, the Acki-Nacki BK reveals a private key, SeqNo, and Merkle Proof in the Verification message. Other BKs must also reveal their private keys eventually. Failure to send Verifications or provide keys accurately results in it being slashed.

### **Proof-of-Stake and Fork Choice rule**

A Fork Choice Rule algorithm based on stake weight to resolve situations where two valid blocks exist at the same height. The algorithm determines which block to finalize by considering attestations and stake amounts. Participants are required to attest to only one block at a certain height, with penalties for attesting to multiple blocks. The protocol involves executing the Acki-Nacki Selection Algorithm for blocks with a certain number of attestations or the highest stake amount. After applying the Fork Choice Rule, the BK sends to other BKs either the block with Attestations or the block with Attestations and Ack/Nack, depending on whether they became Acki-Nacki for that block.

### **Block Finalization**

The process of block finalization involves each BK receiving a new block, updating the state, and marking mutations as not final until receiving Attestations from BP in the Common section of subsequent blocks to meet the minimum Attestation Threshold specified in the network configuration. The BK also waits a specified time for block to finalize after collecting the required number of block Attestations. If no negative acknowledgments (Nack) are received within the time frame, the block is marked as final. If there are insufficient block attestations, the block remains unfinalized, prompting network participants to decide on potential actions such as allowing ongoing unfinalized block production, halting the network after a set number of blocks, or considering penalties for BKs failing to provide attestations.

### **Joint Committee**

In the event of a Nack, multiple Attestations on the same block by one Block Keeper (BK), or other malicious actions, the Joint Committee function is invoked. Each BK is requested to vote for slashing malicious BKs, Block Producers (BPs), or rejecting a malicious block. The network requires as many votes, as determined by network participants, to confirm malicious activity and take appropriate actions.

### **Slashing**

The following are slashing conditions which can lead network participants to either lose their entire stake or a portion of it (bleeding). When we say 'lose,' we mean that the stake will be burned and not transferred to any other party. Burning plays a role in security assumptions.

* An accepted Nack will slash the BP and every Acki-Nacki which sent an Ack for their entire stake.
* Attesting to more than one block at the same height will result in the slashing of the entire stake of the BK.
* Producing two blocks at the same height from the same BP will result in the slashing of the BP's entire stake.
* A non-performing Acki-Nacki will result in the bleeding of its stake.
* A non-performing BK will result in the bleeding of its stake.
* Non-randomized BK keys will result in the bleeding of the stake.
* Non-sequential Acki-Nacki Keys in the Acki-Nacki Merkle Tree will result in the bleeding of the BK's stake.
* Too complex execution of a block by BP will result in the bleeding of its stake.
* Sending a Nack for a valid block will slash Acki-Nacki.
* If a BP did not include transactions from another thread in the right order in block, or included incorrect ones, it will be slashed by entire stake.



### **Dynamically adjustable parameters**

One of the main advantages of the Acki Nacki consensus protocol is presence of several dynamically adjustable parameters such as the number of Attestations needed for block finalization, the average expected number of Acki-Nacki per block, the number of votes for Join Committee and the probability of a successful attack with a certain percentage of malicious BKs. All these parameters can be changed by network participants through voting according to their preferences.

For example, one can input the number of BKs, the desired attack probability with a certain number of malicious BKs. Acki Nacki will then automatically adjust the parameters of the number of Attestations and the number of Acki-Nacki so that the network achieves the highest throughput with the shortest finality.

## **Attacks analysis**

In the analysis of attacks, you have the opportunity to conduct experiments with parameters using the following [link](https://simulator.ackinacki.com/).

Here you can create graphs illustrating the relationship between the probability of a successful attack and the number of Acki-Nacki, comparison the Acki Nacki consensus with the Nakamoto consensus and the BFT consensus, and build a graphs of the Acki-Nacki Consensus input parameters.

### **Performance analysis**

Without taking state sharding into account the limitation to performance in Acki Nacki network is down to the two factors: the number of blocks a BK can receive over the network and apply and the number of blocks all network Verifiers can process at any given moment. This performance is entirely dependent on computer and network resources committed by Participants, number of BKs and expected number of Acki-Nacki per block.

With sharded design there is no theoretical limit to the Acki Nacki network throughput. Without sharded design, taking into account modern computer hardware and datacenter Internet connection we calculate a practical limit of 250,000 transactions per second of a minimal 500 byte messages with less than a 1 sec finality. With sharding enabled the protocol can scale to millions of transactions of any complexity just by adding computing resources which makes it comparable with centralized cloud services.

Acki Nacki achieves this performance as a result of vastly reduced message complexity during most of its operation time.

**The Acki Nacki algorithm achieves consensus in two communication steps.** The first step involves sending the block from the BP to the BKs. The second step involves sending Acks from Acki-Nacki to the BP and Nacks to all BKs, in parallel with the sending of Attestations from the BKs to the BP.

In total, the following messages are sent: the block from BP to BKs, the Attestations from BKs to BP, and the Ack/Nack messages from several chosen Acki-Nacki to BKs. Here, the optimistic scenario ends. The Nack message and accidental Forks will trigger more messages, but as we have shown, Nack messages are highly improbable, and Forks are rare events. Most of the time, the network will operate by sending just 3 types of messages. \
The total number of all messages sent equals **2 \* (N - 1) + v**, where **N** — number of BKs (including BP), **v** — number of Acki-Nacki. The message complexity of Acki Nacki depends on the desired security parameters, taking into account that, in practice, v << N.
