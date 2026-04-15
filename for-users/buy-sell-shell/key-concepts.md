# Key Concepts

Before buying or selling SHELL, it helps to understand the core terms and mechanisms.

## Ecosystem Tokens

### SHELL (ECC\[2])

The primary tradable token. Used for transferring value between different Dapp IDs and subsequent conversion within the Acki Nacki ecosystem to VMSHELL for payment network fees.

### USDC (ECC\[3])

A stablecoin pegged to the US dollar. Used for settlements when buying and selling SHELL. Within the Acki Nacki network, USDC exists as ECC\[3] — an element of the Extra Currency Collection.

### NACKL (ECC\[1])

The native token of the Acki Nacki network. Used for value storage and staking. Can be burned (Redeem) to receive a share of the free USDC reserve.

## Extra Currency Collection (ECC)

The set of currencies within the Acki Nacki network designed for various tasks: paying fees, transferring value, staking. Each currency has its own index: NACKL — 1, SHELL — 2, USDC — 3.

## Exchange Rate

The rate is fixed and immutable:

**1 USDC ecc = 100 SHELL**

{% hint style="info" %}
This means buying 10 USDC worth of SHELL always yields 1,000 SHELL, and selling 5,000 SHELL always yields 50 USDC.
{% endhint %}

## Denominations

A denomination is a fixed lot size when selling SHELL. There are four denominations, similar to banknotes:

| Denomination (USDC ecc) | SHELL Equivalent |
| :---------------------: | :--------------: |
|            1            |        100       |
|            10           |       1,000      |
|           100           |      10,000      |
|          1,000          |      100,000     |

No other denominations exist. When selling SHELL, the wallet automatically breaks down your amount into lots of these denominations.

## Lot

A lot is a single indivisible sell order for SHELL. Each lot is tied to a specific denomination (1, 10, 100, 1,000 USDC ecc) and can only be sold in full. Partial selling of a lot is not possible.

**Example:** a lot with denomination 10 USDC means the seller deposited 1,000 SHELL and is waiting to receive 10 USDC when their turn comes.

## Queue (FIFO)

Each denomination has its own separate queue of lots (4 queues total). Queues work on a First-In-First-Out (FIFO) basis. If you placed your order before others, your lot will be sold first.

## Free Reserve

The free reserve is the USDC held in the contract that is not reserved for any seller. It is formed as follows: when a buyer pays USDC, the system first uses SHELL from seller queues. If there aren't enough sellers, the system creates (mints) the missing SHELL. The USDC received for minted SHELL forms the free reserve.

The free reserve is used exclusively for Redeem NACKL payouts.

## Claim

The process of a seller receiving USDC after their lot has been sold. In Acki Nacki Wallet, this happens automatically in the background.

## Mint

The creation of new SHELL tokens by the system. Occurs when a buyer wants to purchase SHELL but there aren't enough sellers in the queues.

## Redeem (Burn) NACKL

The operation of burning NACKL to receive a share of the free USDC reserve. Irreversible.

## Root (ShellAccumulatorRootUSDC)

The central smart contract of the Accumulator system. Processes SHELL purchases, manages seller queues, performs minting, and handles payouts.

## SellOrderLot (ShellSellOrderLot)

A smart contract representing a single seller lot. Created when a sell order is placed. Self-destructs after USDC payout.
