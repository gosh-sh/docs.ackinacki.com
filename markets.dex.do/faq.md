# FAQ

<details>

<summary><strong>Can I purchase access to a model through the web app without using an AI agent?</strong></summary>

Yes. You can complete the entire purchase process yourself on [markets.dex.do](https://markets.dex.do/): sign up, select a model, fund your Funding Account, and purchase access to the model. After that, you can [configure a Codex CLI](documentation/how-to-set-up-codex-cli.md) profile or [Codex Dekctop](documentation/how-to-set-up-codex-desktop.md) to use the purchased model. \
No AI agent is required. [Learn more here](documentation/how-to-buy-ai-inference-on-dex.do.md)

</details>

<details>

<summary><strong>How can I connect a wallet after signing in with Google if the funding section is not visible?</strong></summary>

In the bottom-left corner of the screen, below the account menu, open the **Spendable balance** section or click the wallet icon.

Then, go to **Funding account** and click **All Funding Accounts**. From there, you can connect your wallet and fund your Funding Account.

</details>

<details>

<summary><strong>When transferring SHELL from Acki Nacki Wallet, I see a message saying that the contract is not deployed and has a <code>NonExist</code> status. Is this normal?</strong></summary>

**Yes, this is normal.** The `NonExist` status means that your Funding Account has not been deployed yet.

To proceed with the transfer:

1. Enter the amount of SHELL you want to transfer.
2. **Do not enable** the automatic token conversion option if it is displayed.
3. Click **Continue** to proceed with the transfer.

</details>

<details>

<summary><strong>Does purchased inference have an expiration date?</strong></summary>

Regular tick purchases have **no fixed expiration date**. You can use the purchased inference until the allocated amount is exhausted, as long as the deal remains open.

Either the buyer or the seller can close the deal early. After final settlement, any remaining buyer funds are returned to their **Trading Wallet**. The seller receives the payment owed to them and the refundable portion of their bond.

The final amounts depend on inference usage, fees, and any applicable deductions specified in the deal terms.

</details>

<details>

<summary><strong>How can I start using inference after my first purchase in the web app?</strong></summary>

After purchasing access to a model through the [markets.dex.do](https://markets.dex.do/) web app, you can configure it for use with [**Codex CLI**](documentation/how-to-set-up-codex-cli.md) or [**Codex Desktop**](documentation/how-to-set-up-codex-desktop.md).

</details>

<details>

<summary><strong>When I go to gosh.ai, I'm prompted to create a Funding Account, even though I already have one. What should I do?</strong></summary>

Make sure you're signed in to [gosh.ai](https://gosh.ai) with the same account you use on [markets.dex.do](https://markets.dex.do). If you're signed in with different accounts, your existing Funding Account will not be displayed.

</details>

<details>

<summary><strong>Can I use multiple purchased models at the same time?</strong></summary>

Yes, you can purchase access to multiple models and use them through [**Codex CLI**](documentation/how-to-set-up-codex-cli.md) or [**Codex Desktop**](documentation/how-to-set-up-codex-desktop.md).

You can use:

* **A  shared API key** (**All current and future models )** — to access all purchased models, including future purchases.
* **Separate API keys** — to access specific models individually.

You can manage your keys in the **Inference API Keys** section in the left-hand menu.

</details>

<details>

<summary><strong>What is a Funding Account?</strong></summary>

A **Funding Account** is a SHELL account used to pay for purchases on [markets.dex.do](https://markets.dex.do/). It is linked to your [**gosh.ai** ](https://gosh.ai/)account.

You top up your Funding Account and select it as the payment source when purchasing model access on markets.dex.do.

When you make a purchase, the required amount is deducted from your Funding Account. This includes the cost of the purchased inference, funding your Trading Wallet, and the wallet creation fee if it has not yet been created.

You can create and use **multiple Funding Accounts**. To create an additional Funding Account, go to **gosh.ai** and click **Create Sub Account**.

</details>

<details>

<summary><strong>What is a Trading Wallet?</strong></summary>

A **Trading Wallet** is a wallet used to pay for inference on DEX.DO.

It is automatically created when you purchase access to a model for the first time and is used for all your purchased models. It is funded from your **Funding Account**.

The wallet has two separate balances:

* **Trading Balance** — funds reserved for payments to inference sellers.
* **Fuel Balance** — funds used to cover the wallet's blockchain transaction fees.

</details>

<details>

<summary><strong>Can a buyer check whether the seller's gateway is publicly accessible before making a purchase?</strong></summary>

No. The seller provides access details for their backend only after the purchase is completed.

If the seller does not respond after the purchase, the buyer can close the order. To do this, go to **Orders** in the left-hand menu, open the relevant order, and click **Stop Order** at the bottom of the page.&#x20;

</details>

<details>

<summary><strong>What does the <code>On Hold</code> status mean? Can I use an order with this status?</strong></summary>

**`On Hold`** means that the order is awaiting fulfillment or is still being processed. The order is not yet ready for use.

Wait until the status changes to the green **Serving** status. Once it does, you can start sending requests to the model.

</details>
