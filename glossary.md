# Glossary

## **A**

#### **Account (contract)**

A record in a distributed database that includes the balance, code, and data.

#### **Ack**&#x20;

A message from the Verifier broadcast to all network participants by Block Verifier (Acki-Nacki) when a block is verified and deemed valid.

#### Acki Nacki Igniter&#x20;

The decentralized network starter protocol (DNSP), it collects Node and [License](glossary.md#license) information, tests and updates the node software and initiates Zerostate (first block) generation once all DNSP requirements are met.

#### **Attestation**

A message sent to the [Block Producer (BP)](glossary.md#block-producer-bp) by any [Block Keeper (BK)](glossary.md#block-keeper-bk) after receiving a block. The Attestation is a BLS signature generated using the BK’s private key. The BP of the next block must aggregate all received Attestations for the previous block into one BLS signature and include it in the Common section of the new block.

## B

#### **Block**

An object containing new transactions, which, once finalized, are stored in the blockchain as a permanent and immutable part of the decentralized ledger. Each block includes transaction data, a timestamp, a common section, and a reference to the previous block, creating a chain of interconnected blocks.

#### **Block Keeper (BK)**&#x20;

A network participant with a deployed [Epoch](glossary.md#epoch) contract that receives blocks from the [Block Producer (BP)](glossary.md#block-producer-bp) and sends back an [Attestation](glossary.md#attestation) containing the block hash and other metadata. A BK can also perform the roles of a [Block Verifier (Acki-Nacki)](glossary.md#block-verifier-or-acki-nacki) or a  BP.

#### **BK Node (BK node)**&#x20;

This is a node that performs the role of a [Block Keeper (BK)](glossary.md#block-keeper-bk).

#### **BK Node Owner**

[BK Node keys](glossary.md#bk-node-owner-keys) owner.

#### **BK Node Owner keys**&#x20;

**(Master keys)**\
The node management keys that provide access to the BK wallet. They are used for managing the node, running node, staking, restaking, and adding licenses to the approved list. These keys cannot be used to withdraw Rewards from the BK wallet.

#### **BK  Reward System**

Block Keepers earn rewards based on their participation in the network during each [Epoch](glossary.md#epoch), regardless of their specific role ([Block Producer](glossary.md#block-producer-bp), [Block Verifier (Acki-Nacki)](glossary.md#block-verifier-or-acki-nacki), or [Block Keeper](glossary.md#block-keeper-bk)). The reward depends on the BK’s [stake](glossary.md#stake) and [Reputation Coefficient](glossary.md#reputation-coefficient), and is evenly distributed among the licenses delegated to it.

#### **BK Wallet**&#x20;

The wallet used for staking. The address of the BK Wallet serves as the identifier of the node.

#### BK Wallet whitelist

A list of [license numbers](glossary.md#license-number) that can be delegated to this node.

#### **Block Manager (BM)**

A network participant whose primary role is to provide users with blockchain data and process external messages. Block Managers receive a portion of the total block reward based on the number of external messages they process.

#### **Block Producer (BP)**&#x20;

The leader of a particular [Thread](glossary.md#thread) responsible for block production.

#### **Block Verifier (or Acki-Nacki)**&#x20;

A Block Keeper responsible for block validation, who notifies all network participants of whether the block is valid or not by sending an [ACK](glossary.md#ack) or [NACK](glossary.md#nack) message.

#### **BLS keys**

The keys used by BK to sign blocks. The lifespan of the keys is one [Epoch](glossary.md#epoch). For a new Epoch, new BLS keys will need to be generated. Each BK stores a list of [BLS](glossary.md#bls-keys) public keys of other BKs (for the current Epoch), which they use to verify [attestations](glossary.md#attestation) on blocks.

## C

#### **Common section**

The section of the block that contains information shared among all network participants.

**Currency Collection**&#x20;

A set of currencies within the Acki Nacki network designed to address various tasks beyond [VMSHELL](glossary.md#vmshell) tokens, which are used for paying fees. These currencies enhance the flexibility and efficiency of the Acki Nacki ecosystem. They provide diverse functionalities tailored to specific use cases, such as value storage and staking ([NACKL](glossary.md#nackl)) or transferring funds between different Dapp IDs for subsequent conversion ([SHELL](glossary.md#shell)).

## D

#### **Dapp ID**

The identifier of a Decentralized Contract System on the Acki Nacki blockchain. This ID is equal to the address of the root smart contract, which is deployed using an external message. All contracts deployed with internal messages automatically receive the same Dapp ID. Whether from the same root contract, or from contracts deployed by the root contract.

## E

#### **Epoch**

The participation period in the Acki Nacki protocol during which a participant acts as a [Block Keeper](glossary.md#block-keeper-bk). It begins immediately after the [Pre-Epoch](glossary.md#pre-epoch).

## L

#### License

The [**BK License**](protocol-participation/license/acki-nacki-node-license.md) is a [contract](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.sol) that grants the right for a BK to participate in the protocol. The license is delegated to a specific BK. Each BK can be delegated up to 20 (twenty) licenses. The Licenses are delegated without restrictions.

#### License number

An **on-chain unique number** assigned during the deployment of the License contract.

#### License Owner

A person who [registered at the Acki Nacki Dashboard](https://docs.ackinacki.com/protocol-participation/license/license-dashboard-guide) and has acquired a [License](https://docs.ackinacki.com/protocol-participation/license/acki-nacki-node-license)  and License Owner Keys to participate in the Acki Nacki protocol.&#x20;

#### License Owner Keys

The keys used to manage license contracts (delegate and revoke delegation) and withdraw staking rewards. They can be obtained by [registering in the dashboard](https://docs.ackinacki.com/protocol-participation/license/license-dashboard-guide).

## M

#### **Minimal Stake**

The minimum amount of tokens a Block Keeper must stake to participate in the network. This value dynamically adjusts based on the difference between the current number of Block Keepers and the required number of Block Keepers in the network.

#### Maximum stake&#x20;

The maximum number of tokens a Block Keeper can stake to participate in the network. This stake remains valid as long as the license stays active. The value is dynamically adjusted based on the current number of Block Keepers in the network, the total number of tokens minted exclusively by Block Keepers, and the number of tokens burned due to slashing penalties.

#### **Mobile Verifier**

A participant in the protocol who occasionally validates transactions in subtrees of accounts. Mobile Verifiers compete in an online game, earning Boosts, to secure a place on the mobile verifiers list, which determines their share of the Block reward.

## N

#### **Nack**&#x20;

A message from the Verifier broadcast to all network participants by Block Verifier (Acki-Nacki) when a block is verified and deemed invalid.

#### **NACKL**&#x20;

The native network token, used for security guarantees: staking, slashing and block rewards.\
(currency collection index: 1)

#### Node Provider&#x20;

A model for providing nodes to license owners, allowing them to participate in the protocol by delegating their licenses to specific nodes. Subsequently, the license owner receives a share of the rewards earned by the [Block Keeper (BK)](glossary.md#block-keeper-bk) for participating in the protocol.

## P

#### **Pre-Epoch**

The period before the [Epoch](glossary.md#epoch) during which a [BK node](glossary.md#block-keeper-node-bk-node) synchronizes states with the network.

#### Proxy

A specialized network service designed to optimize block exchange between participants in the Acki Nacki network. Its primary purpose is to reduce overall network traffic between nodes and to improve the scalability and stability of the network.

## R

#### **Reputation Coefficient**

A metric that increases the rewards for [Block Keepers](glossary.md#block-keeper-bk) based on their continuous participation in the protocol.

## S

#### **SHELL**&#x20;

The utility token within the Acki Nacki network is designed to compensate [NACKL](glossary.md#nackl) holders for the computing resources the network provides. It can be converted to [VMSHELL](glossary.md#vmshell) to cover network fees at a 1:1 ratio. However, it is not possible to convert VMSHELL back to SHELL. This token can be transferred between different [Dapp IDs](glossary.md#dapp-id). (currency collection index: 2)

#### **Stake**

The amount of [NACKL](glossary.md#nackl) tokens required to participate in the Acki Nacki protocol. At the launch of the Acki Nacki network (zerostate), staking is not required if a [License BK](glossary.md#license) is present. [Staking](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#staking) will become necessary for validation at a later stage.

## T

#### **Thread**&#x20;

A subset of nodes that serve a particular subset of [Accounts](glossary.md#account-contract).

## V

#### VMSHELL

The unit of account used to cover network fees. It is converted from [SHELL](glossary.md#shell) and cannot be converted back to SHELL. VMSHELL can be transferred between contracts that belong to the same [Dapp ID](glossary.md#dapp-id).

## Z

#### **Zerostate**

The initial state of the network, representing the first block state of the blockchain. It is created after all the requirements of the [**Decentralized Network Starter Protocol (DNSP)**](https://docs.ackinacki.com/protocol-participation/block-keeper/join-dnsp-gossip) have been fulfilled.
