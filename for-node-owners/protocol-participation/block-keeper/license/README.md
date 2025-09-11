---
description: General Information
---

# License

The [**BK License**](https://docs.ackinacki.com/for-node-owners/protocol-participation/block-keeper/license/acki-nacki-vk-node-license) is a [contract](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.sol) that grants a [Block Keeper (BK) ](../../../../glossary.md#block-keeper-bk)the right to participate in the protocol Acki Nacki.&#x20;

{% hint style="info" %}
To generate rewards, a License must be delegated to a BK node.\
How to delegate is described [here](license-delegation-guide/).
{% endhint %}

Each BK can be delegated up to **20 licenses**. Licenses are delegated without restrictions.

## License status

### Privileged Status and Reputation

From the moment is first delegated, all Licenses have **privileged status**. \
This status allows a BK Node, where all Licenses are privileged, to operate with a stake below the protocol [minimum stake](../../../../glossary.md#minimal-stake-of-the-vk) (“min-stake”).\
If any License on the Node loses privileged status, the Node must meet the full min-stake requirement.\
\
From the moment is first delegated, all Licenses also have a **Reputation Coefficient** =1. \
The coefficient is a multiplier on License rewards and increases gradually with continuous staking, from 1 up to a protocol-defined maximum = 3.\
\
Both privileged status and the reputation coefficient are lost simultaneously if:

* Any part of the stake is withdrawn from the Block Keeper contract system, or
* The License does not participate in validation for longer than half an [Epoch](../../../../glossary.md#epoch-of-the-bk) \
  (259,200 blocks, \~12 hours). \
  This period is measured from the time the stake is placed in the cooler until it reappears in the [Pre-Epoch](../../../../glossary.md#pre-epoch) or subsequent epoch after resuming continuous staking.

**Restaking After Loss**\
To resume staking after loss of privileged status, the License Owner must increase their stake to at least the min-stake requirement.

## Slashing

In the event that a BK node is subject to:

* [**Partial slashing**](../../../../#slashing): the reputation resets to the minimum, and part of the stake is lost.
* [**Full slashing**](../../../../#slashing): the license is permanently destroyed.

## Ownership Transfers

A License may be transferred to another owner **without leaving validation**, provided the License and its staked tokens are transferred together.&#x20;

If the License Owner wishes to withdraw the stake during transfer, the License must first lock its stake, exit validation, withdraw funds, and only then may ownership be changed. In this case, the License loses privileged status and, if it remains outside of validation beyond the permitted period, it also loses reputation. Restaking after such a transfer requires the new owner to meet the min-stake requirement.&#x20;

You can learn more about how to transfer ownership of a License [here](working-with-licenses.md).

## Mitigation of reduced BK rewards caused by a License

* If a stake is withdrawn from the Block Keeper wallet, the corresponding License negatively affects neighbouring Licenses, since rewards are redistributed as (stake ÷ 20 × 19)
* If a license loses its privileged status and the Node’s total stake is below Minimal stake - the Node will not be able to continue staking.
* If the License Owner fails to remove such a License from the Wallet, Node Owner may perform the license lock operation with Node Owner keys to.
* If a License remains outside validation (with locked status) for a prolonged period, it is deemed to reduce the rewards of other Licenses because of the reduced total stake of the Node.
* If a third-party License Owner reduces rewards for others, Node Provider may create a new wallet and migrate all compliant Licenses to it.

How to work with licenses can be found [here](working-with-licenses.md).
