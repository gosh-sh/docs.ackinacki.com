# Purchase with Crypto or Card (Credit/Debit)

Shell Buyer is a web application for purchasing SHELL tokens using a credit/debit card or cryptocurrency. Available at [shellbuy.ackinax.com](https://shellbuy.ackinax.com/).

## How It Works

The purchase consists of three steps:

1. **Connect Wallet** — scan a QR code with your Acki Nacki Wallet to link it
2. **Choose Package** — select a token package and pay with a card or crypto
3. **Confirm & Receive** — SHELL tokens are delivered directly to your wallet

## Prerequisites

* A browser (Chrome, Firefox, Safari, or any modern browser)
* [Acki Nacki Wallet app](https://ackinacki.com/wallet) installed on your phone
* A credit/debit card (Visa, MasterCard) **or** cryptocurrency (USDC, USDT)

## Step-by-Step Guide

{% stepper %}
{% step %}
#### Open Shell Buyer

Navigate to [https://shellbuy.ackinax.com](https://shellbuy.ackinax.com/) in your browser. \
A **Connect Your Wallet** card appears with the **Generate QR Code** button. Click it.

<figure><img src="../../../.gitbook/assets/2.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Scan the QR Code

A QR code is displayed on screen with the message **Scan with Wallet App**. You can either scan the code with your phone's camera or tap **Open in Wallet App** if you are on a mobile device. The page shows "Waiting for wallet confirmation…"

<figure><img src="../../../.gitbook/assets/3.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Confirm Connection in the Wallet

Open your Acki Nacki Wallet app. A prompt appears: **Do you want to connect your wallet to Shell Buyer?** Tap **Connect** to confirm.

<figure><img src="../../../.gitbook/assets/3_1.jpg" alt="" width="188"><figcaption></figcaption></figure>

After confirming, the wallet shows a success screen: **Wallet has been connected**.

<figure><img src="../../../.gitbook/assets/3_3.jpg" alt="" width="188"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Wallet Connected

Back in the browser, the page now shows **WALLET CONNECTED** with your wallet name and address. Two buttons are available: **Browse Packages** and **Disconnect**.

Tap **Browse Packages** to proceed.

<figure><img src="../../../.gitbook/assets/3_2.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Choose Your Packages

The **Choose Your Packages** screen displays five token packages:

<figure><img src="../../../.gitbook/assets/4.jpg" alt="" width="375"><figcaption></figcaption></figure>

Each package has a quantity selector (−/+). You can add multiple packages to your order. The **Order Summary** panel on the right updates in real time, showing the selected packages, total SHELL, and subtotal.

When ready, tap **Proceed to Checkout**.
{% endstep %}

{% step %}
#### Verify Wallet Ownership

The **Checkout** page opens. At the top you'll see a **Wallet verification required** banner

Tap the **Verify Wallet Ownership** button.

<figure><img src="../../../.gitbook/assets/5.jpg" alt="" width="375"><figcaption></figcaption></figure>

The button changes to **Verifying with wallet:**

<figure><img src="../../../.gitbook/assets/5_1.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Approve Verification in the Wallet

In your wallet app, a prompt appears: **Shell Buyer wants to verify your wallet ownership. Confirm?** Tap **Confirm**.

<figure><img src="../../../.gitbook/assets/5_2.jpg" alt="" width="188"><figcaption></figcaption></figure>

After approval, the Checkout page shows a green **Wallet verified** banner.

<figure><img src="../../../.gitbook/assets/5_3.jpg" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="warning" %}
If verification times out, you will see the error "Wait for property. Max 60 attempts reached." \
In this case, tap **Verify Wallet Ownership** again to retry.
{% endhint %}

<figure><img src="../../../.gitbook/assets/5_4_err0r.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Fill in Contact Information

On the Checkout page, fill in:

* **Full Name**
* **Email Address**
* **Country**

{% hint style="info" %}
The **Order Summary** on the right shows your selected packages, SHELL amount, subtotal, any applicable fees and taxes, and the total.
{% endhint %}
{% endstep %}

{% step %}
#### Select Payment Method

{% tabs %}
{% tab title="Credit Card" %}
In the **Payment Method** section, select **Credit Card** (selected by default). The button at the bottom reads **Pay $X.XX with Card**.

Tap **Pay with Card**.

<figure><img src="../../../.gitbook/assets/6 credit_card.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Cryptocurrency" %}
In the **Payment Method** section, select **Crypto**. Additional fields appear:

* **Currency** — choose USDC or USDT

<figure><img src="../../../.gitbook/assets/7_1.jpg" alt="" width="233"><figcaption></figcaption></figure>

* **Network** — choose from Arbitrum, Ethereum, BSC, Polygon, Optimism, or Tron

<figure><img src="../../../.gitbook/assets/7_2.jpg" alt="" width="234"><figcaption></figcaption></figure>

After selecting the currency and network, the button reads **Pay $X.XX with \[currency]**.

Tap the **Pay** button:

<figure><img src="../../../.gitbook/assets/7 crypto (1).jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
#### Complete Payment

{% tabs %}
{% tab title="Credit Card" %}
You are redirected to the payment provider's page (Stripe). Here you can:

* Choose the payment currency (e.g. EUR or USD)
* Enter card details (card number, expiry, CVC, cardholder name)
* Select your country or region
* Optionally use Amazon Pay or other available methods (Bancontact, EPS, etc.)

Complete the payment by tapping **Pay**.

<figure><img src="../../../.gitbook/assets/6_1.jpg" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Payments are securely processed. Card details never touch Shell Buyer servers.
{% endhint %}
{% endtab %}

{% tab title="Cryptocurrency" %}
You are redirected to the NOWPayments page. \
The page displays:

* A **QR code** for the payment address
* The exact **amount** to send (e.g. 10.22142717 USDC)
* The **address** to send to
* A countdown **timer** for the exchange rate lock
* Payment status: Waiting for payment → Processing payment → Success

Send the specified amount of the selected cryptocurrency to the provided address on the correct network.

<figure><img src="../../../.gitbook/assets/7_3.jpg" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="warning" %}
**Important:** send the exact amount shown. The payment must be sent before the timer expires. Payments below the minimum amount cannot be processed. Completed payments are non-refundable.
{% endhint %}

Once the payment is detected, the status changes to **Payment Processing**:

<figure><img src="../../../.gitbook/assets/7_4.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
#### Track Your Order

After payment, the **Order Status** page shows a progress bar with five stages:

1. **PENDING** — order created
2. **PAYMENT CREATED** — payment initiated
3. **PAYMENT CONFIRMED** — payment confirmed by the provider
4. **MINTING** — SHELL tokens are being minted on the Acki Nacki chain
5. **DELIVERED** — tokens have been sent to your wallet

The **Order Details** section displays: Order ID, Status, Amount, SHELL Tokens, Payment Method, Created timestamp, Delivery Transaction link, and Delivered At timestamp.

<figure><img src="../../../.gitbook/assets/7_5.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Order Delivered

When all stages are complete, the status shows **DELIVERED** with confirmation badges: **Minter** ✓ and **On-Chain** ✓. An **On-Chain Verified** timestamp confirms that the tokens have been verified on the Acki Nacki blockchain.

<figure><img src="../../../.gitbook/assets/7_6.jpg" alt="" width="375"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Check Your Wallet

Open your Acki Nacki Wallet. Your SHELL balance now reflects the purchased tokens (e.g. 800 SHELL).

<figure><img src="../../../.gitbook/assets/7_7.jpg" alt="" width="188"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## Disconnecting the Wallet

To disconnect your wallet from Shell Buyer, tap the **Disconnect** button on the Shell Buyer page. \
The wallet app will show a prompt: **Shell Buyer requested disconnection from your wallet. Disconnect now?** \
Tap **Disconnect** to confirm, or **Keep connected** to stay connected.

<div><figure><img src="../../../.gitbook/assets/3_2 (1).jpg" alt="" width="375"><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/3_4.jpg" alt="" width="188"><figcaption></figcaption></figure></div>

## Important Notes

* Payment provider fees (card processing fee, VAT) may be added depending on your country and payment method
* Processing time depends on the payment method: card payments usually take a few minutes, cryptocurrency transfers may take up to 5 minutes after confirmation
* After payment, SHELL minting and delivery happen automatically
* You can track order progress on the Order Status page at any time

## Possible Errors

| Message                                    | Cause                            | Solution                                    |
| ------------------------------------------ | -------------------------------- | ------------------------------------------- |
| Wait for property. Max 60 attempts reached | Wallet verification timed out    | Tap **Verify Wallet Ownership** to retry    |
| Payment failed                             | Payment provider error           | Try again or use a different payment method |
| Wallet connection failed                   | QR code expired or network issue | Generate a new QR code and try again        |
