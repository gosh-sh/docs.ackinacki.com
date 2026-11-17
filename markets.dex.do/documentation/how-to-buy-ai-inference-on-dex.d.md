# How to Buy AI Inference on DEX.D

Sign up for [markets.dex.do](https://markets.dex.do/), fund your **Funding Account** with SHELL, buy access to a model, and configure a Codex CLI profile. This guide uses the `gpt-6.1-sol` model.

{% stepper %}
{% step %}
## Sign up for DEX.DO

Go to [markets.dex.do](https://markets.dex.do/), click **Sign in**, and choose a sign-up method.

![Sign in button on DEX.DO](../../.gitbook/assets/.gitbookassetsdex-do-sign-in.png)

<img src="../../.gitbook/assets/.gitbookassetsdex-do-sign-in-options.png" alt="DEX.DO sign-in options" width="188">
{% endstep %}

{% step %}
## Add funds to your Funding Account

After you sign up, the **Funding accounts** page opens. You use funds from your Funding Account to pay for model ticks. To create the account, make an initial transfer of at least **10 SHELL**.

{% hint style="info" %}
You can top up your Funding Account with SHELL either **by paying with a card** or by transferring funds **from your** [**Acki Nacki Wallet**](https://ackinacki.com/wallet).
{% endhint %}

![Funding accounts page](../../.gitbook/assets/.gitbookassetsfunding-accounts.png)

### Buy SHELL with a card

{% stepper %}
{% step %}
Choose the amount of SHELL to buy in the **Buy SHELL** section.

<img src="../../.gitbook/assets/.gitbookassetsbuy-shell.png" alt="Amount selection in the Buy SHELL section" width="375">
{% endstep %}

{% step %}
Enter your card details and complete the payment.

![Example card checkout form](../../.gitbook/assets/.gitbookassetscard-checkout.png)
{% endstep %}
{% endstepper %}

### Transfer SHELL from Acki Nacki Wallet

{% stepper %}
{% step %}
Open the **I already have SHELL** section.

<img src="../../.gitbook/assets/.gitbookassetsalready-have-shell.png" alt="Funding Account setup details in the I already have SHELL section" width="375">
{% endstep %}

{% step %}
Use the [**Acki Nacki Wallet app**](https://ackinacki.com/wallet) to scan the QR code shown in your account. You can also **copy the address below the QR code** and paste it into the recipient address field in your wallet.

<img src="../../.gitbook/assets/Pasted image 2026-11-16 19.49.44.png" alt="Scan the QR code or copy the address below it to transfer SHELL" height="386" width="127">
{% endstep %}

{% step %}
Transfer at least **10 SHELL**.

\
At this stage, the **Funding Account** has not yet been deployed, so its address will have the status **`NonExist`**. **This is expected behavior.** Once the SHELL transfer is received, the system will automatically deploy the Funding Account and all required contracts.

{% hint style="warning" %}
**If DEV mode** is enabled in your Acki Nacki Wallet, **DO NOT** **check the box** to Auto-convert to VMSHELL
{% endhint %}

<img src="../../.gitbook/assets/.gitbookassetsacki-nacki-wallet-transfer.png" alt="Example transfer of 450 SHELL in the Acki Nacki Wallet app" width="188">
{% endstep %}
{% endstepper %}

{% hint style="info" %}
The initial transfer goes to your **Main Wallet.** DEX.DO automatically sets up your Main Wallet, creates your **Funding Account**, and credits the remaining SHELL to it after deducting setup fees, the Main Wallet reserve, and network fees.
{% endhint %}

<table><thead><tr><th width="313.35003662109375">Amount</th><th>Purpose</th></tr></thead><tbody><tr><td>3 SHELL</td><td>Main Wallet deployment</td></tr><tr><td>2 SHELL</td><td>Main Wallet reserve for future network fees</td></tr><tr><td>3 SHELL</td><td>Funding Account setup</td></tr></tbody></table>

{% hint style="info" %}
You can see your balances in **Spendable balance** in the lower-left corner. \
Click **All funding accounts** to add funds, or click **Manage on gosh.ai** to manage your account on [gosh.ai](https://gosh.ai/).
{% endhint %}

<img src="../../.gitbook/assets/.gitbookassetsmanage-funding-account.png" alt="Manage on gosh.ai and All funding accounts buttons" width="375">
{% endstep %}

{% step %}
## Buy access to the model

In the left sidebar, open **Markets** and select a model. For this example, select `gpt-6.1-sol`.

Start a purchase request and fill in the fields.

<table><thead><tr><th width="235">Field</th><th>Example value</th></tr></thead><tbody><tr><td><strong>Amount</strong></td><td>125 million model tokens (125 ticks). <br>One tick equals 1 million tokens.</td></tr><tr><td><strong>Price per tick</strong></td><td>16 SHELL per tick.</td></tr></tbody></table>

Check the **Estimated transfer amount** and make sure your Funding Account has enough funds. In this example, the order budget is **2,000 SHELL** (125 × 16). The estimated transfer is **2,332 SHELL**, including the fee, buyer bond, and gas reserve.

Click **Create request**. DEX.DO creates a Trading Wallet when you make your first purchase.

<img src="../../.gitbook/assets/.gitbookassetscreate-model-request.png" alt="Purchase request showing the amount, price, and estimated transfer" width="375">

<img src="../../.gitbook/assets/.gitbookassetsbuying-ticks.png" alt="Progress of a tick purchase after submitting the request" width="375">

Wait for the status to change to **Serving** with a green indicator. If the request stays **Pending**, [contact support via chat](https://t.me/c/3819653418/3477).

![Model order with Serving status](../../.gitbook/assets/.gitbookassetsorder-serving.png)

{% hint style="info" %}
DEX.DO uses funds from your Funding Account to top up the **Trading balance** and **Fuel balance**. DEX.DO deducts the funds for model access immediately. You can check your balances in **Spendable balance**.
{% endhint %}

<img src="../../.gitbook/assets/.gitbookassetstrading-and-fuel-balances.png" alt="Trading balance and Fuel balance in the Trading Wallet" width="375">
{% endstep %}

{% step %}
## Configure Codex CLI

To start using your purchased model, configure a **Codex CLI profile** to connect through DEX.DO.

For step-by-step instructions, see [How to Set Up Codex CLI](how-to-set-up-codex-cli.md)
{% endstep %}
{% endstepper %}
