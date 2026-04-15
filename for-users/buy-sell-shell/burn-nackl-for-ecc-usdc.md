# Burn NACKL for ECC USDC

Burn NACKL is a mechanism that lets you burn (permanently destroy) your NACKL tokens and receive a share of the free USDC reserve in return. This is a separate process, unrelated to buying or selling SHELL.

{% hint style="warning" %}
**Irreversible operation:** burned NACKL cannot be recovered. Make sure you want to burn the specified amount.
{% endhint %}

## How It Works

When burning NACKL, you receive a portion of the free USDC reserve proportional to your share of the total NACKL in circulation.

**Formula:**

```
Payout = USDC reserve × (Your NACKL / NACKL total supply)
```

There is no fixed price for NACKL. The value of one NACKL is floating and depends on the size of the reserve and the total supply. The more NACKL you hold relative to the total supply, the larger your share of the reserve.

## Prerequisites

* [Acki Nacki Wallet app](https://ackinacki.com/wallet) installed
* NACKL in your balance

## Step-by-Step Guide

{% stepper %}
{% step %}
#### Open the NACKL Details Screen

On the wallet's main screen, tap the **NACKL** row in the token list.

<figure><img src="../../.gitbook/assets/01 (3).jpg" alt="" width="285"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Tap Burn

The NACKL token screen shows your balance, address, and transaction history. Tap the **Burn** button.

<figure><img src="../../.gitbook/assets/02 (2).jpg" alt="" width="285"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Tap Burn my NACKL

The **Burn NACKL to USDC** screen opens tap the **Burn my NACKL** button to proceed to the amount entry screen

<figure><img src="../../.gitbook/assets/03 (2).jpg" alt="" width="288"><figcaption></figcaption></figure>

You can tap **How it works?** to see an explanation of the mechanism.
{% endstep %}

{% step %}
#### Enter the NACKL Amount

The amount entry screen displays:

* **Select amount** — field to enter the NACKL amount
* **You will receive** — estimated USDC payout, updates in real time (e.g., ≈ 0.000390 USDC)
* **Approximate rate** — current rate (e.g., 1 NACKL ≈ 0.0001300 USDC)
* **How it counts?** — button with calculation details
* **NACKL total supply** — total NACKL in circulation (e.g., 85,546,454)
* **USDC reserve** — size of the free USDC reserve (e.g., 11,122)
* **Your unlocked NACKL** — your available balance with a **Max** button

Enter the amount of NACKL to burn. Tap **Max** to select your entire available balance.

In this example: entering 3 NACKL yields ≈ 0.000390 USDC.

Tap **Burn & Redeem:**

<figure><img src="../../.gitbook/assets/05 (1).jpg" alt="" width="285"><figcaption></figcaption></figure>


{% endstep %}

{% step %}
#### Confirm the Burn

A confirmation screen appears:

Tap **Confirm burn** to proceed or **Cancel** to go back.

<figure><img src="../../.gitbook/assets/06 (3).jpg" alt="" width="288"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Operation Complete

On success, you'll see the **NACKL redeemed** screen.

Tap **Close** to return.

<figure><img src="../../.gitbook/assets/07 (3).jpg" alt="" width="286"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Check the Result

After burning:

* Your NACKL balance decreased (was 10, now 7)
* The NACKL transaction history shows a **Sent to Accumulator** entry with amount -3

<figure><img src="../../.gitbook/assets/08 (2).jpg" alt="" width="285"><figcaption></figcaption></figure>

Your NACKL balance on the main wallet screen updates automatically.

<figure><img src="../../.gitbook/assets/09 (3).jpg" alt="" width="281"><figcaption></figcaption></figure>

The received USDC can be seen on the USDC token screen in the **Transaction history** section — a **Received from Accumulator** entry with amount +0.00039.

<figure><img src="../../.gitbook/assets/10 (1).jpg" alt="" width="285"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## What Is the Free Reserve?

The free reserve is the USDC held in the contract that is not reserved for SHELL sellers. It is formed as follows: when a buyer purchases SHELL but there aren't enough sellers in the queues, the system creates (mints) new SHELL. The USDC paid for minted SHELL forms the free reserve. Seller funds are never touched.

## When Is Redeem Unavailable?

Redeem is not possible when the free reserve is zero. This means all USDC in the contract is reserved for SHELL sellers. The reserve grows as SHELL is purchased and there aren't enough sellers in the queues.

## Possible Errors

| Message                          | Cause                  | Solution                     |
| -------------------------------- | ---------------------- | ---------------------------- |
| Burn & Redeem button inactive    | Field is empty or zero | Enter the NACKL amount       |
| Insufficient NACKL balance       | Not enough NACKL       | Reduce the amount            |
| No USDC available for redemption | Free reserve is 0      | Wait for the reserve to grow |
| Transaction failed               | Transaction error      | Try again                    |
