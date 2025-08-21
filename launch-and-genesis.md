---
description: Joining the Acki Nacki Network from the Genesis Block
---

# Launch & Genesis

To launch the network, **Igniter DNSP clients** collect data that will be used to generate the **Zerostate**.\
At the time of network launch, the Zerostate will include:

* All **BK wallets** along with their corresponding **whitelists**
* All **license contracts** and mappings to the **Node Owners’ wallet contracts** to which those licenses were delegated

#### ✅ Before the network launch:

* All **License Owners** must delegate all licenses they want to be included in the Zerostate.\
  👉 [See the Delegation Instruction](for-node-owners/protocol-participation/block-keeper/license/license-delegation-guide/)
* All **Node Owners** must:
  * Generate the necessary key pairs
  * Collect **all required signatures**
  * Properly configure the **Igniter DNSP client**
  * Run the Igniter for each node\
    👉 See the [Igniter Setup Guide](https://github.com/ackinacki/ackinacki/blob/main/README.md)

***

#### 🚨 Important:

* The **Igniter** must be running **for each node** using the **same server IP** and **Proxy IP** that will be used for deploying the BK node later.
* The **same keys and licenses** used during Igniter setup must also be used when deploying the BK.

***

#### ⏱ Network Launch Timing

The **Gosh team will announce the exact network launch time in advance** (including day, hour, and minute).\
So, you must either:

* Set up an **automatic launch** of the deployment scripts and staking on each server where the Igniter was run\
  **OR**
* Launch them **manually at the specified time**

👉 [BK Deployment Instructions](https://app.gitbook.com/o/-McrzPd05fQMWkwCo65G/s/yPWnJ1v4nIPgOrEhPVrK/)\
👉 [Proxy Deployment Instructions](https://github.com/gosh-sh/acki-nacki?tab=readme-ov-file#proxy)
