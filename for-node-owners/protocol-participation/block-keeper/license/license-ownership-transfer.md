---
description: >-
  This guide explains how to transfer BK licenses to another owner through the
  Acki Nacki Dashboard, either in bulk or by selecting specific licenses.
---

# License Ownership Transfer

You can transfer Block Keeper (BK) licenses to another owner directly through the [Acki Nacki Dashboard](https://dashboard.ackinacki.com/licenses).

You can transfer multiple licenses at once or select specific licenses by their IDs.

**Important:**

* Transferring a license changes ownership of the BK license contract, including its staked tokens.
* The recipient must be another registered Dashboard user. Transfers to contract addresses are not supported.
* If a license is already linked to a BK node, you do not need to unlink it before transferring ownership.

{% hint style="info" %}
Transferring license ownership is different from moving a license from one BK node to another. If you want to move licenses between nodes without changing their owner, follow the separate guide on transferring BK licenses between nodes.
{% endhint %}

### Step 1. Open the Transfer Licenses Panel

1. Open the [Acki Nacki Dashboard](https://dashboard.ackinacki.com/licenses) and sign in.
2. Go to the **Licenses** tab.
3. Click **Actions** and select **Transfer**.

<figure><img src="../../../../.gitbook/assets/1 (4).jpg" alt=""><figcaption></figcaption></figure>

4. In the **Transfer licenses** panel, select **Block Keeper** from the **Node Type** dropdown.

<figure><img src="../../../../.gitbook/assets/2 (5).jpg" alt="" width="375"><figcaption></figcaption></figure>

### Step 2. Select Licenses to Transfer

There are two ways to select licenses.

#### Option A: Transfer by Quantity

Use this option to transfer a specified number of licenses without selecting individual license IDs.

1. Select the **Quantity** tab.
2. Check the number of available licenses displayed under **Available**.
3. Enter the number of licenses you want to transfer in the **Quantity** field.

The Dashboard automatically selects licenses in a fixed order from all your licenses of the selected type.

{% hint style="warning" %}
**For now, we recommend transferring licenses in small batches of around 10 at a time.**
{% endhint %}

<figure><img src="../../../../.gitbook/assets/3 (5).jpg" alt="" width="375"><figcaption></figcaption></figure>

**Important:** If you need to transfer particular licenses, use **Specific licenses** instead to avoid transferring unintended licenses.

#### Option B: Transfer Specific Licenses

Use this option to choose exactly which licenses to transfer.

1. Select the **Specific licenses** tab.
2. Click **Choose licenses** to open the list of licenses you own.
3. Use the **Search by license ID** field to find a license if needed.
4. Check the boxes next to the licenses you want to transfer.
5. Verify your selection.

The **Selected** counter shows how many licenses you have chosen. You can click **Clear** to reset your selection.

<figure><img src="../../../../.gitbook/assets/4 (6).jpg" alt="" width="375"><figcaption></figcaption></figure>

### Step 3. Enter the Recipient's Public Key

In the **Recipient's license owner public key** field, enter the public key of the new license owner.

**Important:**

* The public key must belong to another registered Dashboard user.
* Enter the recipient's **license owner public key**, not a BK node address or smart contract address.
* Double-check the public key before proceeding to ensure the licenses are transferred to the intended owner.

### Step 4. Confirm the Transfer

1.  Check the confirmation box:

    _I confirm that I am transferring control of the selected BK license contracts and their stake to the new owner._
2. Enter your Dashboard passcode in the **Enter passcode** field.
3. Click **Transfer**.

### Step 5. Monitor Transfer Progress

The Dashboard displays the transfer progress, including:

* **Transferred** — the number of successfully transferred licenses.
* **Cancelled** — the number of cancelled transfers.
* **Progress bar** — the overall transfer progress.
* **Individual license statuses** — the status of each license in the transfer list.

Possible statuses include:

* **Waiting to transfer** — the license is waiting to be processed.
* **Transferred** — the license has been successfully transferred.

For bulk transfers, licenses are processed individually.

To stop transferring licenses that have not yet been processed, click **Cancel remaining**.

**Important:** Cancelling the remaining transfers does not reverse licenses that have already been transferred.

<figure><img src="../../../../.gitbook/assets/6 (3).jpg" alt="" width="375"><figcaption></figcaption></figure>

### Step 6. Verify the Transfer

Once the operation is finished, the Dashboard displays **Transfer completed**, along with the transfer summary and individual license results.

1. Verify that the expected licenses show **Transferred**.
2. Click **Close**.
3. Ask the recipient to open their Dashboard and verify that the transferred licenses appear under their **Licenses** tab and that the **Available** counter shows the expected number of licenses.<br>

<figure><img src="../../../../.gitbook/assets/7 (6).jpg" alt="" width="375"><figcaption></figcaption></figure>
