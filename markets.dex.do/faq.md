# FAQ

<details>

<summary><strong>Can I purchase access to a model through the web app without using an AI agent?</strong></summary>

Yes. You can complete the entire purchase process yourself on [markets.dex.do](https://markets.dex.do/): sign up, select a model, fund your Funding Account, and purchase access to the model. After that, you can [configure a Codex CLI](documentation/how-to-set-up-codex-cli.md) profile or [Codex Dekctop](documentation/how-to-set-up-codex-desktop.md) to use the purchased model.\
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

* **A shared API key** (**All current and future models )** — to access all purchased models, including future purchases.
* **Separate API keys** — to access specific models individually.

You can manage your keys in the **Inference API Keys** section in the left-hand menu.

{% hint style="info" %}
Purchasing access to the same model again will automatically close the current deal. Once the deal is settled, the remaining funds will be returned to your **Trading Wallet** in SHELL, minus any applicable deductions.\
You can view it under **Spendable balance → Trading wallet → Activity**
{% endhint %}

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

If the seller does not respond after the purchase, the buyer can close the order. To do this, go to **Orders** in the left-hand menu, open the relevant order, and click **Stop Order** at the bottom of the page.

</details>

<details>

<summary><strong>What does the <code>On Hold</code> status mean? Can I use an order with this status?</strong></summary>

**`On Hold`** means that the order is awaiting fulfillment or is still being processed. The order is not yet ready for use.

Wait until the status changes to the green **Serving** status. Once it does, you can start sending requests to the model.

</details>

<details>

<summary><strong>What happens to my unused tokens if I place another order for the same model?</strong></summary>

A new order closes your current deal. Unused tokens don’t carry over. For example, if you have 2 million tokens left and buy another 5 million, you’ll have 5 million tokens available once the new deal starts. \
Access to the model may pause briefly during the switch.

After the previous deal settles, any remaining funds are returned to your Trading Wallet in SHELL, minus any applicable deductions.

</details>

<details>

<summary><strong>What do the letters in the order book’s Type column mean?</strong></summary>

The letters show which rules apply to an offer.&#x20;

{% hint style="info" %}
When buying inference on **markets.dex.do**, you choose how many ticks to buy and the maximum price you’ll pay per tick.\
**1 tick equals 1 million tokens.**
{% endhint %}

<table data-header-hidden data-search="false"><thead><tr><th width="108.5"></th><th width="266.60009765625"></th><th></th></tr></thead><tbody><tr><td>Type</td><td>Meaning</td><td>Can I buy this offer?</td></tr><tr><td>No letter</td><td><strong>Regular limit order.</strong> You can buy all or part of the listed amount.</td><td>Yes</td></tr><tr><td><strong>A</strong></td><td><strong>All-or-none.</strong> You must buy the full amount in one deal. For example, a 10-tick offer requires a 10-tick purchase.</td><td>Yes, in full</td></tr><tr><td><strong>P</strong></td><td><strong>Post-only.</strong> The seller placed the offer in the book to wait for a buyer. Once listed, it’s available to buy.</td><td>Yes</td></tr><tr><td><strong>T</strong></td><td><strong>Trusted Execution Environment (TEE).</strong> The offer specifies a protected environment for running inference.</td><td>Currently unavailable</td></tr><tr><td><strong>S</strong></td><td><strong>Subscription.</strong> The offer is for a subscription.</td><td>Currently unavailable</td></tr><tr><td><strong>?</strong></td><td><strong>Unknown type.</strong> The dashboard can’t identify all the rules for this offer.</td><td>Unavailable</td></tr></tbody></table>

{% hint style="info" %}
An offer can show more than one letter. \
For example, **A P** means the seller posted the offer to the book and you must buy it in full.
{% endhint %}

</details>

<details>

<summary><strong>What does my order status mean?</strong></summary>

**Orders can have the following statuses:**

| Status              | What it means                                                                       |
| ------------------- | ----------------------------------------------------------------------------------- |
| **Payment pending** | Your payment is awaiting confirmation.                                              |
| **Queued**          | Your purchase is waiting to be processed.                                           |
| **Funding**         | The service is preparing funds in your Trading Wallet for the purchase.             |
| **Processing**      | The service is setting up your deal.                                                |
| **Serving**         | The model is ready to accept inference requests.                                    |
| **Partially ready** | Some inference slots in your order are already accepting requests.                  |
| **Updating**        | An inference slot is being updated.                                                 |
| **On hold**         | Your order needs attention. Open the order to see the details.                      |
| **Closing**         | The deal is being closed.                                                           |
| **Filled**          | The order has ended. This status also appears when the deal is stopped or replaced. |

</details>

<details>

<summary><strong>What are the <code>taker fee reserve</code> and the <code>buyer bond</code>?</strong></summary>

The **taker fee reserve** is the amount set aside to cover the marketplace fee. You’re the _taker_ when you buy from an existing sell order. \
For example, the order budget is **20 SHELL**, so the 2.5% fee reserve is **0.5 SHELL**. \
Any unused portion returns to your Trading Wallet when the deal settles.

The **buyer bond** is a security deposit held for the duration of the deal. It equals twice the price of one tick. At **2 SHELL per tick**, the bond is **4 SHELL**. The full bond returns to your Trading Wallet under normal settlement. A dispute or breach of the deal terms may result in deductions.

</details>

<details>

<summary><strong>Where can I view my Trading Wallet transaction history?</strong></summary>

In the lower-left corner of the dashboard, open the **Spendable balance** menu, and click the balance in the **Trading wallet** section. Under **Activity**, you will see your recent transactions. To view the full history, click **View full activity** and select the **Trading wallet** filter.

The transaction history includes deposits, transfers, funds reserved for purchases, collateral, and refunds. Click any transaction to view its amount and details.

</details>

<details>

<summary><strong>Where can I check my token usage and remaining SHELL?</strong></summary>

Open **Inference keys** in the dashboard, select an active API key, and scroll down to the **Usage** section.

* **Remaining** — SHELL available for inference requests.
* **Total tokens** also includes tokens read from and written to the cache.
* **Cost** — SHELL spent over the selected period.

You can view the number of requests, token usage, and costs for today, all time, by model, or by day.

To update the statistics, click **Refresh**.

</details>

<details>

<summary><strong>How can I check my connection to a Seller?</strong></summary>

Go to **Pools**, expand the relevant model, and click **Connection details** under **Seller status**. Then click **Test connection**.

The slot must be **Ready** and have an active deal. The test may take up to five minutes and consumes purchased tokens.

If the result is **OK**, the connection is working. If the test fails, see the **Seller connection** window for details.

</details>

<details>

<summary><strong>How can I give my app’s users access to models on markets.dex.do?</strong></summary>

Store your API key on your backend. Set up a proxy that checks each user’s permissions, forwards authorized requests to markets.dex.do using your API key, and returns the model’s response to your app. Users sign in through your app, and the API key stays on your server.

{% hint style="info" %}
By default, each model slot supports **up to two concurrent requests**. \
All users whose requests go through the same slot share this limit. Idle sessions do not count toward it. A streaming request occupies one spot until the response ends.
{% endhint %}

</details>
