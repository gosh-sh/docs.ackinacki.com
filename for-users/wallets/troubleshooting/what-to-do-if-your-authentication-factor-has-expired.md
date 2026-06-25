---
description: >-
  This guide explains how to refresh an expired authentication factor in AN
  Wallet
---

# What to Do if Your Authentication Factor Has Expired

If AN Wallet shows **Security factor expired**, you need to refresh the ZK factor by signing in again with the same wallet name and the same social account.

<figure><img src="../../../.gitbook/assets/ZK factor exp.jpg" alt="" width="364"><figcaption></figcaption></figure>

While the security factor is expired, wallet operations will not work. This also affects connected features that depend on the wallet, including mining keys.

{% hint style="info" %}
**The ZK factor lets AN Wallet confirm that you signed in with the correct social account without publicly linking that account to your wallet on-chain**. When the factor is valid, the wallet can use it for operations without asking you to complete the full social sign-in flow each time. When it expires, you need to sign in again to refresh it.
{% endhint %}

### Before You Sign Out

{% hint style="danger" %}
**Before signing out, make sure you know your seed phrase and that it is saved in a secure place.**\
You may need the seed phrase to restore access to your wallet.
{% endhint %}

<figure><img src="../../../.gitbook/assets/photo_2026-06-25_17-24-01.jpg" alt="" width="317"><figcaption></figcaption></figure>

### Refresh the Authentication Factor

#### 1. Sign Out of the Wallet

Open the AN Wallet settings and sign out by tapping "Log Out" at the bottom of the screen.

<figure><img src="../../../.gitbook/assets/1.jpg" alt="" width="317"><figcaption></figcaption></figure>

***

#### 2. Sign In Again

Launch the AN Wallet flow again. On the sign-in screen, select the option "Recover existing wallet".

<figure><img src="../../../.gitbook/assets/3.jpg" alt="" width="315"><figcaption></figcaption></figure>

***

#### 3. Sign In with the Same Wallet Details

Use exactly the same wallet name that you used before signing out, enter the wallet password, and sign in with the same social account that were used for this wallet (Google, Facebook or Telegram).

<div><figure><img src="../../../.gitbook/assets/3.1.jpg" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/3.2.jpg" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/3.3.jpg" alt=""><figcaption></figcaption></figure></div>

***

#### 4. Confirm the New ZK Factor

After successful sign-in, AN Wallet will set up a fresh ZK authentication factor for your wallet.

<figure><img src="../../../.gitbook/assets/4.jpg" alt="" width="325"><figcaption></figcaption></figure>

### Result

Your authentication factor is refreshed. You can continue using the wallet with the same wallet name and the same social authentication factor.
