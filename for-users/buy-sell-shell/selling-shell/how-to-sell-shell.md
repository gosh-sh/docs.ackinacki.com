# How to Sell SHELL

Selling/converting SHELL is done through the Acki Nacki Wallet. You choose a lot denomination, confirm — and your SHELL is locked until sold. When a buyer arrives, the lot is sold automatically on a first-come-first-served (FIFO) basis.

{% hint style="warning" %}
**Important:** once a sell order is confirmed, cancellation is not possible.\
Your SHELL will remain locked until sold.
{% endhint %}

## Prerequisites

* [Acki Nacki Wallet app](https://ackinacki.com/wallet) installed
* SHELL in your balance (minimum 100 SHELL for the smallest lot)

## Step-by-Step Guide

{% stepper %}
{% step %}
#### Open the **Convert** Section

On the main wallet screen, where your balances are displayed, tap the **Swap** button button and select **Convert SHELL**

<div><figure><img src="../../../.gitbook/assets/1 (3).jpg" alt="" width="188"><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/2 (4).jpg" alt="" width="188"><figcaption></figcaption></figure></div>
{% endstep %}

{% step %}
#### Select **Send SHELL**

On the conversion screen, make sure the **`-`** mode is selected.

<figure><img src="../../../.gitbook/assets/3 (4).jpg" alt="" width="188"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Choose a Lot Denomination

The panel will show which lots are available based on your balance.

The **Convert SHELL** panel opens with the prompt "Choose how much eccUSDC you want to receive." \
\
Four denominations are available:

|   You SHELL   |  You Receive  |
| :-----------: | :-----------: |
|   100 SHELL   |   1 eccUSDC   |
|  1,000 SHELL  |   10 eccUSDC  |
|  10,000 SHELL |  100 eccUSDC  |
| 100,000 SHELL | 1,000 eccUSDC |

Denominations that exceed your SHELL balance are unavailable. Your current SHELL balance is displayed at the bottom of the screen.

Tap the desired denomination. \
In this example, the **1,000 SHELL → 10 eccUSDC** lot is selected:

<figure><img src="../../../.gitbook/assets/4 (3).jpg" alt="" width="188"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Confirm the conversion

A confirmation screen appears:

<figure><img src="../../../.gitbook/assets/5 (4).jpg" alt="" width="188"><figcaption></figcaption></figure>

Tap **Confirm** to proceed or **Cancel** to go back.
{% endstep %}

{% step %}
### Order Placed

After confirmation, you'll see a screen with the order details:

* **Sell:** 1,000 SHELL
* **Receive:** 10 eccUSDC
* **Position in queue:** #1

The queue number shows how many lots of this denomination are ahead of you. The lower the number, the sooner your lot will be sold.

Tap **Close** to return.

<figure><img src="../../../.gitbook/assets/6 (2).jpg" alt="" width="188"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Check the Result

After placing the order:

* Your SHELL balance decreased (was 55,000, now 54,000 — 1,000 SHELL is locked)
* The SHELL token screen now shows a **My Orders** section with your lot and its queue position

<figure><img src="../../../.gitbook/assets/7 (1).jpg" alt="" width="188"><figcaption></figcaption></figure>

Your SHELL balance on the main wallet screen updates automatically.

<figure><img src="../../../.gitbook/assets/8 (1).jpg" alt="" width="188"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## How to Exchange a Larger Amount

Each transaction creates one lot of one denomination. To convert more SHELL, simply repeat the process multiple times, choosing the denominations you need.

**Example:** you want to exchange 15,400 SHELL (= 154 eccUSDC). Create lots:

1. 1 × 100,000 SHELL (= 1,000 eccUSDC) — if you have enough balance, or skip
2. 1 × 10,000 SHELL (= 100 eccUSDC)
3. 5 × 1,000 SHELL (= 5 × 10 eccUSDC)
4. 4 × 100 SHELL (= 4 × 1 eccUSDC)

Each lot joins its denomination's queue independently and is sold separately.

## Denominations Work Like Banknotes

The system uses four fixed denominations, similar to paper bills. A lot can only be sold in full — partial selling is not possible.

Choosing a denomination is a trade-off:

* **Smaller denominations** (1, 10 eccUSDC) — sell faster, as even small purchases can fill them
* **Larger denominations** (100, 1,000 eccUSDC) — fewer transactions needed, but may take longer to find a buyer

## Transaction History

On the SHELL token screen, the **Transaction history** section shows all operations:

* **Sent to Accumulator** — SHELL locked when placing an order to convert SHELL to eccUSDC.
* **Received from Accumulator** — SHELL received as a result of converting eccUSDC to SHELL.

<figure><img src="../../../.gitbook/assets/7 (4).jpg" alt="" width="188"><figcaption></figcaption></figure>

## What Happens Next

After placing your order, your lot waits in the queue. Learn more about status tracking: [Tracking Your Orders](tracking-your-orders.md). About receiving eccUSDC after the sale: [Receiving USDC (Claim)](/broken/pages/4a68c01959125f6dcd554e0a91ed8733b8b21b6f).

## Possible Errors

| Message                               | Cause                                  | Solution                      |
| ------------------------------------- | -------------------------------------- | ----------------------------- |
| Denomination unavailable (grayed out) | Not enough SHELL for this denomination | Choose a smaller denomination |
| Transaction failed. Try again         | Transaction error                      | Try again                     |
