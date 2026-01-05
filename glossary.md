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

#### **Bee Engine**

An embeddable client-side mining engine and blockchain verification system for background **NACKL** mining, providing cryptographic validation of computation results and reward distribution based on verifiable user contribution (see [**Bee Engine Integration**](https://dev.ackinacki.com/bee-engine/integration))

#### **Block**

An object containing new transactions, which, once finalized, are stored in the blockchain as a permanent and immutable part of the decentralized ledger. Each block includes transaction data, a timestamp, a common section, and a reference to the previous block, creating a chain of interconnected blocks.

#### **Block Keeper (BK)**&#x20;

A network participant with a deployed [Epoch](glossary.md#epoch) contract that receives blocks from the [Block Producer (BP)](glossary.md#block-producer-bp) and sends back an [Attestation](glossary.md#attestation) containing the block hash and other metadata. A BK can also perform the roles of a [Block Verifier (Acki-Nacki)](glossary.md#block-verifier-or-acki-nacki) or a  BP.

#### **BK Node (BK node)**&#x20;

This is a node that performs the role of a [Block Keeper (BK)](glossary.md#block-keeper-bk).

#### **BK Node Owner**

[BK Node keys](glossary.md#bk-node-owner-keys) owner.

#### **BK Node Owner keys**&#x20;

The node management keys that provide access to the BK wallet. They are used for managing the node operations such as staking and adding licenses to the approved list. These keys cannot be used to withdraw Rewards from the BK wallet.

#### **BK  Reward System**

Block Keepers earn rewards based on their participation in the network during each [Epoch](https://docs.ackinacki.com/glossary#epoch), regardless of their specific role ([Block Producer](https://docs.ackinacki.com/glossary#block-producer-bp), [Block Verifier (Acki-Nacki)](https://docs.ackinacki.com/glossary#block-verifier-or-acki-nacki), or [Block Keeper](https://docs.ackinacki.com/glossary#block-keeper-bk)). The reward depends on the [stake](https://docs.ackinacki.com/glossary#stake) and [Reputation Coefficient](https://docs.ackinacki.com/glossary#reputation-coefficient), and is evenly distributed among the licenses delegated to it.

#### **BK set**

A set of Block Keepers from which a Block Producer is selected for each [thread](glossary.md#thread).\
Block Keepers are added to the BK set from the [Future BK set](glossary.md#future-bk-set).\
The members of the BK‑set ensure the operation of the network: they send [attestations](glossary.md#attestation), validate blocks, and issue [Ack](glossary.md#ack)/[Nack](glossary.md#nack) signals.\
The size of the BK‑set directly affects the network’s security parameters, defining how many attestations are required to confirm blocks and the percentage of [Block Verifiers (or Acki‑Nacki)](glossary.md#block-verifier-or-acki-nacki) needed for each block.

#### **BK Wallet**&#x20;

The wallet used for staking. The address of the BK Wallet serves as the identifier of the node.

#### BK Wallet whitelist

A list of [license numbers](glossary.md#license-number) that can be delegated to this node.

#### **Block Manager (BM)**

A network participant whose primary role is to provide users with blockchain data and process external messages. Block Managers receive a portion of the total block reward based on the number of external messages they process. A BM operates in conjunction with a specific BK, and this pairing is defined during start of BM.

#### **BM  Reward System**

The amount of NACKL tokens accrued for processing BM external messages during a single Epoch. The reward should be claimed once per Epoch, but it can only be claimed **after** the slashing period (approximately after 5% of the next epoch’s blocks have been processed).

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

#### **Epoch of the BK**

The participation period in the Acki Nacki protocol during which a participant acts as a [Block Keeper](glossary.md#block-keeper-bk). It begins immediately after the [Pre-Epoch](glossary.md#pre-epoch) and lasts for 259,200 blocks (about 24 hours). The duration of a specific BK’s epoch may be extended if the number of BKs in the network is lower than required.

#### **Epoch of the BM**

The participation period in the Acki Nacki protocol during which a participant acts as a [Block Manager](glossary.md#block-manager-bm). The length of an epoch is 259 200 blocks (about 24 hours).

## F

#### Future BK set

A list of prospective [Block Keepers (BKs)](glossary.md#block-keeper-bk) for whom a [Pre‑Epoch](glossary.md#pre-epoch) contract has already been deployed.\
This list is used for pre‑approving access to the main [BK set](glossary.md#bk-set).\
After the [Epoch](glossary.md#epoch) contract is deployed, the future BK must be approved by the current BK set to be added to the active BK set. Otherwise, it will be removed from the Future BK set.

## L

#### License

The [**BK License**](for-node-owners/protocol-participation/block-keeper/license/acki-nacki-vk-node-license.md) is a [contract](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.sol) that grants the right for a Block Keeper (BK) to participate in the protocol. The license is delegated to a specific BK. Each BK can be delegated up to 20 (twenty) licenses. The Licenses are delegated without restrictions. &#x20;

The **BM License** is a [contract](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/LicenseBM.sol) that grants the Block Manager (BM) the right to participate in the protocol. Each BM License is delegated to  BM wallet _(1 wallet - 1 license)_.

#### License number

An **on-chain unique number** assigned during the deployment of the License contract.

#### License Owner

A person who [registered at the Acki Nacki Dashboard](https://docs.ackinacki.com/protocol-participation/license/license-dashboard-guide) and has acquired a [License](https://docs.ackinacki.com/protocol-participation/license/acki-nacki-node-license)  and License Owner Keys to participate in the Acki Nacki protocol.&#x20;

#### License Owner Keys

The keys used to manage license contracts (delegate and revoke delegation) and withdraw staking rewards. They can be obtained by [registering in the dashboard](https://docs.ackinacki.com/protocol-participation/license/license-dashboard-guide).

## M

#### **Minimal Stake of the BK**

The minimum amount of tokens a Block Keeper must stake to participate in the network. This value dynamically adjusts based on the difference between the current number of Block Keepers and the required number of Block Keepers in the network.

**Minimal Stake of the BM**

The minimum amount of tokens locked on Block Manager wallet.\
It is calculated individually and represents a percentage of the total NACKLs earned by that BM.\
The percentage depends on the time elapsed since the network launch.\
&#xNAN;_(If a BM stops operating in the middle of an_ [_Epoch_](glossary.md#epoch-of-the-bm)_, no reward is accrued for that Epoch.)_

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

An off-chain entity that for provides  [Block Keeper (BK) nodes](glossary.md#bk-node-bk-node) to license owners, allowing them to participate in the protocol by delegating their licenses to specific nodes. Node Provider keys are used to sign and validate the delegation to a particular Node Provider before Network start to form the zerostate, they do not operate on-chain. Subsequently, the license owner receives a share of the rewards earned by the BK for participating in the protocol

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

The amount of [NACKL](glossary.md#nackl) tokens required to participate in the Acki Nacki protocol. It is calculated as the total sum of all stakes from licenses delegated to a specific node. At the launch of the Acki Nacki network (zerostate), staking is not required if a [License BK](glossary.md#license) is present. [Staking](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#staking) will become necessary for validation at a later stage.

## T

#### **Thread**&#x20;

A subset of nodes that serve a particular subset of [Accounts](glossary.md#account-contract).

## V

#### VMSHELL

The unit of account used to cover network fees. It is converted from [SHELL](glossary.md#shell) and cannot be converted back to SHELL. VMSHELL can be transferred between contracts that belong to the same [Dapp ID](glossary.md#dapp-id).

## Z

#### **Zerostate**

The initial state of the network, representing the first block state of the blockchain. It is created after all the requirements of the [**Decentralized Network Starter Protocol (DNSP)**](https://docs.ackinacki.com/protocol-participation/block-keeper/join-dnsp-gossip) have been fulfilled.
