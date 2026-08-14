# Ways to Buy SHELL

There are three ways to purchase SHELL tokens. The choice depends on your payment method and whether you prefer a graphical application or direct smart-contract interaction.

## Option 1: Accumulator Contract — Exchange eccUSDC Directly

**For:** advanced users and operators who already have eccUSDC (ECC\[3]) and want to submit the exchange transaction with `tvm-cli`.

**Interface:** the Accumulator smart contract on Mainnet/Shellnet.

**How it works:**

1. Prepare a wallet transaction containing a whole-number amount of eccUSDC
2. Send the transaction to the Accumulator contract with the required VMSHELL value
3. The contract exchanges eccUSDC at the fixed rate of 1 eccUSDC = 100 SHELL
4. SHELL is credited to the sending contract or to a recipient encoded in the payload

Detailed guide: [Exchange eccUSDC for SHELL via the Accumulator](exchange-eccusdc-for-shell-via-accumulator.md)

***

## Option 2: Shell Buyer App — Pay with Card or Crypto



**For:** users who want to buy SHELL using fiat currency (credit/debit card) or cryptocurrency (USDT, USDC).

**Application:** [Shell Buyer App](https://shellbuy.ackinax.com/) — a web application accessible through any browser. A mobile version is planned for the future.

**How it works:**

1. Open the Buy Shell App
2. Connect your Acki Nacki Wallet
3. Choose a payment method (card or cryptocurrency)
4. Enter the amount
5. Payment is processed through the payment provider
6. SHELL is credited to your wallet

Detailed guide: [Buy via Shell Bayer App](https://shellbuy.ackinax.com/)

***

## Option 3: Acki Nacki Wallet — Pay with eccUSDC

**For:** users who already have eccUSDC in their balance on the Acki Nacki wallet.

**Application:** [Acki Nacki Wallet (mobile app)](https://ackinacki.com/wallet).

**How it works:**

1. Open the Buy/Sell SHELL section in the wallet
2. Enter the eccUSDC amount
3. Confirm the transaction
4. SHELL is instantly credited to your balance

Detailed guide: [Buy via Acki Nacki Wallet](exchange-eccusdc-for-shell-via-acki-nacki-wallet-android-only.md)

***

## Which Method Should I Choose?

* **Already have eccUSDC and prefer a graphical interface?** Use the Acki Nacki Wallet — it's instant and has no additional fees.
* **Need to exchange eccUSDC programmatically?** Send the exchange transaction directly to the Accumulator contract.
* **Have a bank card or crypto on another blockchain?** Use the Shell Buyer App — it lets you buy SHELL without needing to acquire eccUSDC first.
