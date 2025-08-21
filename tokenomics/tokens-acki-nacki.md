---
hidden: true
---

# Tokens Acki Nacki

Acki Nacki uses a unique **Binary Token System**, consisting of two native tokens — [**NACKL**](../glossary.md#nackl) and [**SHELL**](../glossary.md#shell) — each with distinct roles in the network’s economy:

## NACKL

**NACKL** is the core value-holding and staking token of the Acki Nacki ecosystem.&#x20;

It is:

* **Deflationary**, with a minting curve identical to Bitcoin’s issuance model — but smoothed.
* **Used for staking**, securing the network via Proof-of-Stake (PoS).
* **Burnable** to access deposited USD stablecoins in the **Accumulator Contract**.
* Rather than halving every 4 years like Bitcoin, NACKL supply **decreases gradually each block**.

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="563"><figcaption></figcaption></figure>

**NACKL is decentralized**: no ICO, no TGE, no IDO — 100% fair distribution and PoS-based value system.

## **SHELL**

**SHELL** is a utility token made **exclusively for use within the Acki Nacki ecosystem**. It’s **not meant for speculation**, and its price **never goes above $1**.&#x20;

**How it works:**

* To mint **100 SHELL**, you deposit **1 USD stablecoin** (like USDC or USDT) into a smart contract.
* You can use SHELL to compensate NACKL holders for the computing resources the network provides. It can be converted to [VMSHELL](../glossary.md#vmshell) to cover network fees at a 1:1 ratio.
* **You can’t redeem SHELL back into stablecoins directly**, but you can:
  * Sell it on the open market
  * Return it to the **Mint Pool** (a network mechanism)

> SHELL is **not a stablecoin** — it’s not pegged, not algorithmic, and not redeemable. But its value is always **equal to or less than $1**.

**📜 Regulatory-Friendly**

SHELL is designed to be **fully compliant** with regulations like the **EU MiCA** and **Digital Asset Act**, since it doesn’t behave like a traditional stablecoin and makes no promise of redemption.for network fees or payments.

### VMSHELL&#x20;

**VMSHELL** is the token used to **pay for network fees** (like gas).

* It is created by **converting SHELL** into VMSHELL.
* **You can’t convert it back** to SHELL after that — it’s one-way.
* VMSHELL can be **transferred between smart contracts** that are part of the **same Dapp** (same [Dapp ID](../glossary.md#vmshell)).

> Think of VMSHELL like prepaid gas credits for your app — once converted, it’s only for powering activity inside the network.

## 🔁 Token Interactions

Here's how the Binary System works in practice:

<table><thead><tr><th width="246.800048828125">Action</th><th width="456.14990234375">Description</th></tr></thead><tbody><tr><td>💵 Deposit 1 USD stablecoin</td><td>→ Mint 100 SHELL</td></tr><tr><td>🧠 Use SHELL</td><td>→ Pay for compute resources on-chain using VMSHELL</td></tr><tr><td>🔁 Burn NACKL</td><td>→ Claim proportional USD from the Accumulator</td></tr><tr><td>⚙ Protocol Participants (Node Owners)</td><td>→ Earn NACKL by providing compute services</td></tr></tbody></table>

#### The Accumulator Contract

* All USD stablecoins used to mint SHELL are held in the **Accumulator Contract**.
* Only NACKL holders can **access these funds** — by **burning their tokens proportionally**.
* Example: Hold 1% of all NACKL,  Burn them to withdraw 1% of the stablecoins in the Accumulator. However NACKL holders rarely (more likely never) will burn for USD (would lose future value). NACKL supply decreases naturally, reinforcing value.
* USD stablecoins are the "floor" for NACKL, but the "ceiling" for SHELL. NACKL's market value always exceeds all USD in the accumulator contract, as the market always predicts higher future utility.

This setup creates a natural **value floor** under NACKL and incentivizes **holding** over redeeming.

#### Token Comparison Table

| TOKENs                | NACKL                                                                                                | SHELL                                                                                                                                                | VMSHELL                                                                                                     |
| --------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **How it's created**  | <p>- Earned as rewards for network participation (staking, validating)<br>- Bought on the market</p> | <p>- Minted by depositing fiat or crypto (USD stablecoins)<br>- Fixed rate: 1 USD stable = 100 SHELL</p>                                             | - Converted from SHELL (1:1)                                                                                |
| **What you can do**   | <p>- Stake<br>- Earn rewards<br>- Restake<br>- Trade on the market</p>                               | <p>- Transfer between different DApps and DApp IDs<br>- Convert to VMSHELL to pay for network/contract fees<br>- Deposit back into the Mint Pool</p> | <p>- Pay for compute/network fees<br>- Can only be transferred between contracts with the same Dapp ID.</p> |
| **What happens next** | - Accumulates in user wallets (no forced burning or expiration)                                      | <p>- Does not expire or burn automatically<br>- May be deposited back or traded</p>                                                                  | - Burned after use                                                                                          |

This architecture ensures a sustainable, non-speculative, and self-reinforcing crypto economy — built for long-term scalability and decentralization.
