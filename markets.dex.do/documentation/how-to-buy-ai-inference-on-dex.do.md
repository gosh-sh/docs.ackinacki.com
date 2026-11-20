# How to Buy AI Inference on DEX.DO

Sign up for [markets.dex.do](https://markets.dex.do/), fund your **Funding Account** with SHELL, buy access to a model, and configure a Codex CLI profile. This guide uses the `gpt-6.1-sol` model.

{% embed url="https://drive.google.com/file/d/1Av1Kq2u_XTRCrOr5ecpfoEmyNWNQr-aH/view" %}

{% stepper %}
{% step %}
## Sign up for DEX.DO

Go to [markets.dex.do](https://markets.dex.do/), click **Sign in**, and choose a sign-up method.

![Sign in button on DEX.DO](../../.gitbook/assets/.gitbookassetsdex-do-sign-in.png)

<img src="../../.gitbook/assets/.gitbookassetsdex-do-sign-in-options.png" alt="DEX.DO sign-in options" width="188">
{% endstep %}

{% step %}
## Choose a model and an offer

In the left sidebar, open **Markets**. It lists every model with tokens on offer, its market price per tick, and its volume.

![Markets list on DEX.DO](../../.gitbook/assets/dex-do-buy-markets.png)
{% endstep %}

{% step %}
## Check the price

The buy panel shows what the order costs and how many credits it needs.

**Order price breakdown** is the order itself:

* **Order budget** — ticks × max price per tick: 10 × 2 = 20 SHELL.
* **Taker fee reserve** — 2.5% of the budget: 0.5 SHELL.
* **Buyer bond** — the price of 2 ticks: 4 SHELL.
* **Order total** — 24.5 SHELL.

Your first purchase also deploys your **Trading Wallet** and creates your **Funding Account**, so the card below it reads **Required credits to deploy wallet and buy tokens**. Click a line to see its parts:

* **Spendable top-up** — the Trading Wallet needs at least 100 SHELL. It pays for this order (24.5 SHELL); the rest stays for future orders.
* **Fuel reserve** — 250 SHELL of Trading Wallet fuel for network operations, plus 8 SHELL to set up the Funding Account.
* **Total credits required amount** — rounded up to whole dollars, $4 at least: **400 SHELL ($4.00)** here. What the rounding adds stays spendable in your Funding Account.

<img src="../../.gitbook/assets/dex-do-buy-first-estimate.png" alt="Order price breakdown and the credits a first purchase requires" width="375">

{% hint style="info" %}
On a short screen, **Order price breakdown** folds into one line with the order total. Tap it to see the lines.
{% endhint %}

Click **Add credits to deploy wallet and buy**.
{% endstep %}

{% step %}
## Add credits

**Add credits** opens with the amount your order needs. Choose **Buy by Card** or **Deposit from Wallet**.

### Buy by card

{% stepper %}
{% step %}
Check the amount: **400 SHELL ($4)**, what you get, and the total. VAT is added at checkout. Click **Pay** under the total.

<img src="../../.gitbook/assets/dex-do-buy-add-credits-card.png" alt="Add credits: buy 400 SHELL by card" width="560">
{% endstep %}

{% step %}
Enter your card details on Stripe Checkout and complete the payment.

![Example card checkout form](../../.gitbook/assets/.gitbookassetscard-checkout.png)
{% endstep %}

{% step %}
Wait while DEX.DO buys your SHELL. The page shows each step: **Paying on Stripe**, **Delivering SHELL to your gosh.ai account**, **Creating your funding account**, and **Adding SHELL to dexdo-buy**. This can take a few minutes.

<img src="../../.gitbook/assets/dex-do-buy-buying-shell.png" alt="Progress of a SHELL purchase by card" width="560">
{% endstep %}

{% step %}
When **SHELL added** appears, your Funding Account **dexdo-buy** holds the SHELL: 400 SHELL minus the 8 SHELL setup. Click **Back to buying ticks**.

<img src="../../.gitbook/assets/dex-do-buy-shell-added.png" alt="SHELL added to the dexdo-buy Funding Account" width="560">
{% endstep %}
{% endstepper %}

### Deposit from your Acki Nacki Wallet

{% stepper %}
{% step %}
Choose **Deposit from Wallet**. The page asks you to **Send 400 SHELL** and shows a QR code and an address.

* Scan the QR code with the [**Acki Nacki Wallet app**](https://ackinacki.com/wallet): the amount is already filled in.
* Or click **Copy** next to the address and paste it into the recipient field in your wallet.
* On a phone, tap **Pay with AN Wallet** — or the QR code itself — to open the wallet with the amount filled in.

<img src="../../.gitbook/assets/dex-do-buy-deposit-send.png" alt="Deposit from Wallet: QR code, address, and the amount to send" width="560">

<img src="../../.gitbook/assets/dex-do-buy-deposit-phone.png" alt="Deposit from Wallet on a phone with the Pay with AN Wallet button" width="188">
{% endstep %}

{% step %}
Send the whole amount from your wallet.

At this stage, the **Funding Account** has not yet been deployed, so its address will have the status **`NonExist`**. **This is expected behavior.** Once the SHELL transfer is received, the system will automatically deploy the Funding Account and all required contracts.

{% hint style="warning" %}
**If DEV mode** is enabled in your Acki Nacki Wallet, **DO NOT** **check the box** to Auto-convert to VMSHELL
{% endhint %}

<img src="../../.gitbook/assets/.gitbookassetsacki-nacki-wallet-transfer.png" alt="Example transfer of 450 SHELL in the Acki Nacki Wallet app" width="188">
{% endstep %}

{% step %}
Once the whole amount has arrived, DEX.DO sets up your main wallet, creates your Funding Account **dexdo-buy**, and transfers the available balance to it.

<img src="../../.gitbook/assets/dex-do-buy-deposit-setup.png" alt="Setting up the main wallet and the Funding Account" width="560">
{% endstep %}

{% step %}
When **Ready** appears, click **Back to buying ticks**.

<img src="../../.gitbook/assets/dex-do-buy-deposit-ready.png" alt="Funding Account ready after the deposit" width="560">
{% endstep %}
{% endstepper %}

Either way, DEX.DO deducts **8 SHELL** to set up your accounts. The rest goes to **dexdo-buy**.

{% hint style="info" %}
Your Funding Account balance is shown in **Spendable balance** in the lower-left corner. Open it to see your Trading Wallet and Funding Accounts, click **All funding accounts** to add funds, or click **Manage on gosh.ai** to manage your account on [gosh.ai](https://gosh.ai/).
{% endhint %}

<img src="../../.gitbook/assets/dex-do-buy-balance-menu.png" alt="Spendable balance menu with the Funding Account, Manage on gosh.ai, and All funding accounts" width="375">
{% endstep %}

{% step %}
## Buy access to the model

**Back to buying ticks** returns you to the same model with the same order.

Under **Order total**, the line **+ Wallet deploy** shows what deploying your Trading Wallet adds to this first order: 325.5 SHELL — the rest of the 100 SHELL minimum (75.5 SHELL, which stays on your Trading Wallet for future orders) and 250 SHELL of fuel. DEX.DO pays for both from your Funding Account.

Click **Create request**. DEX.DO creates a Trading Wallet when you make your first purchase.

<img src="../../.gitbook/assets/dex-do-buy-create-request.png" alt="Purchase request showing the amount, the price, the order total, and the wallet deploy line" width="375">

The panel shows the purchase step by step: **Creating the request**, **Paying from dexdo-buy**, **Queued for the model**, and **Setting up the model**. You can close the panel — the purchase carries on, and you can follow it in **Orders**.

<img src="../../.gitbook/assets/dex-do-buy-buying-ticks.png" alt="Progress of a tick purchase" width="375">

When **Ticks bought** appears, the model is ready. Click **Continue to API key**, or wait a few seconds for it to open by itself.

<img src="../../.gitbook/assets/dex-do-buy-ticks-bought.png" alt="Ticks bought: the model, the ticks, the price, and what was paid" width="375">

Wait for the status to change to **Serving** with a green indicator. If the request stays **Pending**, [contact support via chat](https://t.me/c/3819653418/3477).

![Model order with Serving status](../../.gitbook/assets/.gitbookassetsorder-serving.png)

{% hint style="info" %}
DEX.DO uses funds from your Funding Account to top up the **Trading balance** and **Fuel balance**. DEX.DO deducts the funds for model access immediately. You can check your balances in **Spendable balance**.
{% endhint %}

<img src="../../.gitbook/assets/dex-do-buy-trading-wallet.png" alt="Trading balance and Fuel balance in the Trading Wallet" width="320">
{% endstep %}

{% step %}
## Get an API key

On [markets.dex.do](https://markets.dex.do/), open **Inference keys**, select **All current and future models**, or choose a key for a specific model, and click **Copy** next to the key in the panel that opens.&#x20;

<figure><img src="../../.gitbook/assets/изображение.png" alt=""><figcaption><p>List of keys in the Inference keys section</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/изображение (1).png" alt="" width="375"><figcaption><p>Desktop</p></figcaption></figure>

Use this key to configure Codex CLI and Codex Desktop.
{% endstep %}

{% step %}
## Configure Codex CLI or Desktop&#x20;

To start using your purchased model, configure **Codex CLI or Codex Desktop** to connect to DEX.DO.

For step-by-step instructions, see [How to Set Up Codex CLI](how-to-set-up-codex-cli.md) or [How to Set Up Codex Desktop](how-to-set-up-codex-desktop.md)
{% endstep %}
{% endstepper %}

## How can a buyer close an order?

If the seller stops responding, you can close your order by following these steps:

1. Go to **Orders** in the left-hand menu.
2. Open the order you want to close.
3. Click **Stop order** at the bottom of the panel.
4. Review the closing conditions and click **Stop order** again to confirm.

The order status will change to **Closing**. Click **Refresh** to check the status.

Once all associated deals are confirmed closed, the order status will change to **Filled**, and the message **The deal is closed** will appear in the order details.

Any remaining funds will be returned to your **Trading Wallet** after final settlement and applicable deductions. They will **not** be automatically returned to your Funding Account.

{% hint style="info" %}
The **Stop order** button is available only when the order has deals eligible to be stopped. If the button is not visible, refresh the order status. If you are still unable to close the order, contact support.
{% endhint %}
