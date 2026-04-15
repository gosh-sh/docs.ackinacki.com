# Receiving ECC USDC (Claim)

When your lot is sold, ECC USDC becomes available for collection. In the Acki Nacki Wallet, the claim process happens **automatically**.

## How It Works

1. The wallet periodically checks the status of your lots
2. When a lot transitions to **Sold** status, the wallet automatically initiates the ECC USDC claim
3. ECC USDC is credited to your balance
4. The lot transitions to **Claimed** status
5. The lot's receipt contract self-destructs after payout

{% hint style="info" %}
**Automatic claim** works in the background while the wallet is open. \
**You just need to open the SHELL details on the main screen.**
{% endhint %}

## What Happens at the Smart Contract Level

For a deeper understanding, here's how the claim works technically:

1. The wallet sends a `Claim()` command to your lot contract (`ShellSellOrderLot`)
2. The `ShellSellOrderLot` verifies the call comes from the owner
3. \`ShellSellOrderLot\` sends a `ClaimUSDC` request to the main contract (`ShellAccumulatorRootUSDC`)
4. \`ShellAccumulatorRootUSDC\` verifies:
   * The lot has actually been sold (it's within the sold prefix of the queue)
   * The request comes from a genuine lot contract (address verification)
   * There is an outstanding debt to pay
5. \`ShellAccumulatorRootUSDC\` transfers ECC USDC directly to you (the lot owner)
6. The lot contract confirms receipt and self-destructs

## If Claim Happens Too Early

It's possible that the wallet attempts to claim ECC USDC before the lot has actually been sold. In this case:

* The transaction is rejected by the Root contract
* The lot contract automatically resets its status via the `onBounce` mechanism
* The wallet will retry later

This is completely transparent to you and requires no action on your part.

## Lots Receive ECC USDC Independently

Each lot is an independent entity. If you created 8 lots, each one will be sold and paid out separately. You'll start receiving ECC USDC as each individual lot sells, without waiting for all of them to sell.

**Example:** you sold 5,300 SHELL (8 lots). An hour later, 3 lots of 10 ECC USDC each are sold — you receive 30 ECC USDC. The remaining 5 lots continue waiting in their queues.
