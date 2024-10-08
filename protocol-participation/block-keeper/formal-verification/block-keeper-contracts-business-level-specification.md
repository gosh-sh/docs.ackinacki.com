---
description: Prepared by Pruvendo
---

# Block Keeper Contracts Business-Level Specification

<div align="right">

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

</div>

## Purpose <a href="#docs-internal-guid-9795c783-7fff-46c9-b911-6f08cd34ef9c" id="docs-internal-guid-9795c783-7fff-46c9-b911-6f08cd34ef9c"></a>

The purpose of the present document is to create business-level specification (highest-level of specification) for the Block Keeper set of smart contracts. This document is intended to:

* Be thoroughly reviewed by the Customer
* Act as a base for the high-level specification

## Introduction <a href="#docs-internal-guid-5550335e-7fff-89ab-4f37-277c3ef4ff70" id="docs-internal-guid-5550335e-7fff-89ab-4f37-277c3ef4ff70"></a>

The key intention of the _Block Keeper_ set of smart contracts is to let any participant become a _Block Keeper_ either for a single participation (in Acki Nacki protocol as Block Keeper) period (called _Epoch_) or repeatedly.

### Single participation period <a href="#docs-internal-guid-ce3f9b2b-7fff-ec17-ca31-cd55d9f52bfc" id="docs-internal-guid-ce3f9b2b-7fff-ec17-ca31-cd55d9f52bfc"></a>

In this case the following stages are introduced:

* _Pre Epoch_. This stage is relatively short (a few minutes) and intended to let the _Block Keeper_ to finish synchronizing its node if necessary
* _Epoch_ - the main stage where the participant acts as a _Block Keeper_. Its duration is usually is most than one day
* _Cooling_ - the terminating stage where the _Block Keeper_ can still be slashed (up to 30% of Epoch’s duration)

The workflow described below must be followed:

* Participant must be registered as a _Block Keeper_
* Stake (exceeding minimally allowed value, which is a dynamic parameter) must be deposited
* _Pre Epoch_ must be started and, then, finished
* Then, _Epoch_:
  * Must be started
  * Throughout the _Epoch_, the participant is a _Block Keeper_
  * Later _Epoch_ must be finished
* Then, Cooling:
  * Must be started
  * At any time the _Block Keeper_ can be slashed
  * Later _Cooling_ must be finished and:
  * Then, stake can be withdrawn or reinvested

Graphically, the described workflow can be presented by the following chart:

<figure><img src="../../../.gitbook/assets/p1.jpg" alt=""><figcaption><p>Single participation period</p></figcaption></figure>

### Repeated participation <a href="#docs-internal-guid-f718a19a-7fff-f6ed-34b0-ca7b74dab25c" id="docs-internal-guid-f718a19a-7fff-f6ed-34b0-ca7b74dab25c"></a>

The main difference with the Single Participation Period is that upon _Epoch_ completion, a new _Epoch_ starts immediately.

The workflow described below must be followed:

* Participant must be registered as a _Block Keeper_
* Stake (exceeding minimally allowed value) must be deposited
* _Pre Epoch_ must be started and, then, finished
* Then, _Epoch_:
  * Must be started
  * **Another (additional) stake must be deposited making the workflow repeated thus turning Epoche into continue state**
  * Later _Epoch_ must be finished
* Then:
  * New _Epoch_ (in non-continued state) starts immediately with additional stake
  * Cooling:
    * Must be started with the rest of stake
    * At any time the _Block Keeper_ can be slashed
    * Later Cooling must be finished and:
    * Then, stake can be withdrawn or reinvested any time in future

The described workflow is illustrated by the following diagram:

<figure><img src="../../../.gitbook/assets/p2.jpg" alt=""><figcaption><p>Repeated participation</p></figcaption></figure>

### Stake workflow <a href="#docs-internal-guid-920047f6-7fff-4afa-4d1e-b3f9cb182c05" id="docs-internal-guid-920047f6-7fff-4afa-4d1e-b3f9cb182c05"></a>

Throughout the loop, stakes are transferred from one entity to another. For single participation their workflow can be illustrated by the following diagram (the second row indicated what entity the stake belongs to at each stage):

<figure><img src="../../../.gitbook/assets/p3.jpg" alt=""><figcaption><p>Stake workflow (single participation period)</p></figcaption></figure>

For repeated participation the diagram is more complicated:

<figure><img src="../../../.gitbook/assets/p4.jpg" alt=""><figcaption><p>Stake workflow (repeated participation period)</p></figcaption></figure>

It’s important to mention that in case the participant (owner) reinvests each stake for the repeated participation immediately after cooling down by continuing the current _Epoch_, he gets some kind of a carousel of continuous participation.&#x20;



Such a carousel is illustrated in the diagram below. For simplicity, all the “gray” periods between ending of the particular stages and external actions are omitted. Green dots stay for the “first” stake, while the red one - for the “second” stake.

<figure><img src="../../../.gitbook/assets/p5.jpg" alt=""><figcaption></figcaption></figure>

### Reward <a href="#docs-internal-guid-1f906f17-7fff-a48a-cfe6-45b9f1f96bf1" id="docs-internal-guid-1f906f17-7fff-a48a-cfe6-45b9f1f96bf1"></a>

Reward is provided by external request sent any time (probably, multiple times) throughout the Epoch. It is implemented using a special TVM instruction and depends on:

* Epoch duration
* Stake
* Total Stake
* Signature
* Reputation time (cumulative time of being a _Block Keeper_)

## Details <a href="#docs-internal-guid-241e8147-7fff-2b71-1594-f76f0c85dd1a" id="docs-internal-guid-241e8147-7fff-2b71-1594-f76f0c85dd1a"></a>

### Common

Before any action any participating system contract must be supplied with gas enough to perform this action, if necessary

### Participant registration

* External request comes from anybody
* New unique _BNW_ (_Block Keeper_ Node Wallet) is created
* Newly created _BNW_ is initialized

### Stake placement <a href="#docs-internal-guid-527ca074-7fff-b8e2-0967-ad93d18961ed" id="docs-internal-guid-527ca074-7fff-b8e2-0967-ad93d18961ed"></a>

* Owner sends a request to _BNW_
* _BNW_ checks if:
  * Owner is correct
  * Stake value is available (in ECC that serves as a secondary currency)
  * In case of success  _Root_ is informed
* Root checks if:
  * Stake exceeds minimally allowed value, otherwise it must be sent back
  * In case of success _BNW_ is informed
* New BKPE (Block Keeper Pre Epoch) created
* Stake is moved to _Pre Epoch_

### Pre Epoch <a href="#docs-internal-guid-072d00ba-7fff-d7da-3a08-843eea10601e" id="docs-internal-guid-072d00ba-7fff-d7da-3a08-843eea10601e"></a>

* Upon creation:
  * _BKPE_ is initialized
  * _BNW_ is moved to “Pre Epoch” state
* Then, at any moment external _touch_ message can be received
* If time for the next stage still did not come, nothing happens
* Otherwise:
  * New _Epoch_ created
  * Stake is moved to _Epoch_
  * _BKPE_ is self-destructed and all its assets are transferred to _Epoch_

### Epoch <a href="#docs-internal-guid-b806b8c9-7fff-cf98-1cdf-4823854ad73b" id="docs-internal-guid-b806b8c9-7fff-cf98-1cdf-4823854ad73b"></a>

*   Upon creation:

    * _Epoch_ is initialized
    * _Root_ is informed to enlist the Block Keeper
    * _BNW_ is moved to “Epoch” state


* Then, at any moment external touch message can be received
* Also, at any moment external slash message can be received, and:
  * The whole stake will be confiscated
  * No reward is granted
  * The participant is blacklisted
* If time for the next stage still did not come, nothing happens
* Otherwise:
  * _Root_ is informed
  * Root checks if it’s possible to end an _Epoch_ and does nothing if negative
  * Otherwise:
    * _Block Keeper_ is delisted
    * _Epoch_ is informed and:
      * _Cooler_ is created
      * Stake (with reward) is moved to _Cooler_
      * _Epoch_ is self-destructed in favor of _Cooler_

### Cooler <a href="#docs-internal-guid-029786ee-7fff-6d20-a43b-199fc279c076" id="docs-internal-guid-029786ee-7fff-6d20-a43b-199fc279c076"></a>

* Upon creation:
  * _Cooler_ is initialized
  * _BNW_ is moved to “Cooler” state
* Then, at any moment external _touch_ message can be received
* Also, at any moment external _slash_ message can be received, and:
  * The whole stake will be confiscated
  * No reward is granted
  * The participant is blacklisted
* If time for the next stage still did not come, nothing happens
* Otherwise:
  * _BNW_ is moved to initial state
  * _BKPE_ is self-destructed and all stake transferred to the _BNW_

### Epoch (Repeated) <a href="#docs-internal-guid-fa3beb56-7fff-55e9-3101-6bf90e843ac5" id="docs-internal-guid-fa3beb56-7fff-55e9-3101-6bf90e843ac5"></a>

* Upon creation:
  * _Epoch_ is initialized
  * _Root_ is informed to enlist the _Block Keeper_
  * _BNW_ is moved to “Epoch” state
* Then, at any moment **additional stake** request can be received with continue flag, making the epoch repeated
* Then, at any moment external _touch_ message can be received
* Also, at any moment external _slash_ message can be received, and:
  * The whole stake will be confiscated
  * No reward is granted
  * The participant is blacklisted
* If time for the next stage still did not come, nothing happens
* Otherwise:
  * _Root_ is informed
  * Root checks if it’s possible to end an _Epoch_ and does nothing if negative
  * Otherwise:
    * _Epoch_ is informed and:
      * New repeated _Epoch_ is created with **additional stake**
      * _Cooler_ is created is the rest of stake with reward
      * _Epoch_ is self-destructed in favor of _Cooler_

## Top-level scenarios <a href="#docs-internal-guid-059b7248-7fff-d1d3-7bd1-55ff014bcf77" id="docs-internal-guid-059b7248-7fff-d1d3-7bd1-55ff014bcf77"></a>

To formally specify the behavior briefly described above, the following top-level scenarios were identified for further creation of high-level and, then, low-level formal specification:

* Registration
* ECC Withdrawal
* Participation

