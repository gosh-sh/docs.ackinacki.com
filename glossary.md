# Glossary

## **A**

#### **Account (contract)**

A record in a distributed database that includes a balance, code, and data.

#### **Ack**&#x20;

A message from the Verifier broadcast to all network participants by Block Verifier (Acki-Nacki) when a block is verified and deemed valid.

#### **Attestation**

A message sent to the Block Producer (BP) by any Block Keeper (BK) after receiving a block. The Attestation is a BLS signature generated using the BK’s private key. The Block Producer of the next block must aggregate all received Attestations for the previous block into one BLS signature and include it in the Common section of the new block.

## B

#### **Block**

An object containing new transactions, which, once finalized, are stored in the blockchain as a permanent and immutable part of the decentralized ledger. Each block includes transaction data, a timestamp, a common section, and a reference to the previous block, creating a chain of interconnected blocks.

#### **Block Keeper (BK)**&#x20;

A network participant that receives blocks from the Block Producer (BP) and sends back an Attestation with the block hash and other metadata. A Block Keeper can also become a Block Verifier (Acki-Nacki) or a Block Producer (BP).

#### **Block Keeper Node (BK)**&#x20;

A node with a deployed Epoch contract that participates in the Acki Nacki protocol.

#### **Block Keeper  Reward System**

Block Keepers earn rewards based on their participation in the network during each _epoch_, regardless of their specific role (Block Producer, Block Verifier (Acki-Nacki), or Block Keeper). The reward depends on the BK’s stake and Reputation Coefficient.

#### **Block Manager**&#x20;

A network participant whose primary role is to provide users with blockchain data and process external messages. Block Managers receive a portion of the total block reward based on the number of external messages they process.

#### **Block Producer (BP)**&#x20;

The leader  of a particular Thread responsible for block production.

#### **Block Verifier (or Acki-Nacki)**&#x20;

A Block Keeper responsible for block validation, who notifies all network participants of whether the block is valid or not.

## C

#### **Common section**

The section of the block that contains information shared among all network participants.

## D

#### **Dapp ID**

The identifier of a Decentralized Contract System on the Acki Nacki blockchain. This ID is equal to the address of the root smart contract, which is deployed using an external message. All contracts deployed with internal messages from the same root contract, or from contracts deployed by the root contract, automatically receive the same DAPP ID.

## E

#### **Epoch**

The participation period in the Acki Nacki protocol during which a participant acts as a Block Keeper.

## M

#### **Minimal Stake**

The minimum amount of tokens a Block Keeper must stake to participate in the network. This value dynamically adjusts based on the difference between the current number of Block Keepers and the required number of Block Keepers in the network.

#### **Mobile Verifier**

A participant in the protocol who occasionally validates transactions in subtrees of accounts. Mobile Verifiers compete in an online game, earning Boosts, to secure a place on the mobile verifiers list, which determines their share of the Block reward.

## N

#### **Nack**&#x20;

A message from the Verifier broadcast to all network participants by Block Verifier (Acki-Nacki) when a block is verified and deemed invalid.

#### **NACKL**&#x20;

The native network token, used for security guarantees: staking, slashing and block rewards.

#### **Node Owner**&#x20;

The owner of staked funds, who holds full rights to manage the Block Keeper wallet, including withdrawing funds and performing other operations.

## R

#### **Reputation Coefficient**

A metric that increases the rewards for Block Keepers based on their continuous participation in the protocol.

## S

#### **Service Key**

An additional public key that provides limited access to the Block Keeper wallet, allowing only Block Keeper node operations. This key is added by the owner.

#### **Stake**

The amount of NACKL tokens required to participate in the Acki Nacki protocol.

#### **SHELL**&#x20;

The utility token within the Acki Nacki network is designed to compensate NACKL holders for the computing resources the network provides. It can be converted to VMSHELL to cover network fees at a 1:1 ratio. However, it is not possible to convert VMSHELL back to SHELL. SHELL can be transferred between different Dapp IDs.

## T

#### **Thread**&#x20;

A subset of nodes that serve a particular subset of Accounts.

## V

#### VMSHELL

The unit of account used to cover network fees. It is converted from SHELL and cannot be converted back to SHELL. VMSHELL can be transferred between contracts that belong to the same Dapp ID.
