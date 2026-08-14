# Exchange eccUSDC for SHELL via Accumulator

This guide explains how to exchange [eccUSDC](../../../glossary.md#usdc-ecc) for SHELL by sending an internal message from a user wallet to the Accumulator contract.

## Prerequisites

* [tvm-cli](https://github.com/tvmlabs/tvm-sdk/releases)
* A deployed `UpdateCustodianMultisigWallet_v2` wallet; the Acki Nacki Wallet app cannot submit this raw contract transaction
* The wallet's full `<WALLET_DAPP_ID>::<WALLET_ACCOUNT_ID>` address and the keys file required to sign transactions
* The `UpdateCustodianMultisigWallet_v2.abi.json` ABI file available locally
* At least `1 eccUSDC` (ECC currency ID `3`); only whole eccUSDC amounts can be exchanged
* More than `0.1 VMSHELL` in the wallet to cover the message value and transaction fees

## Summary

To perform the exchange, send the following to the Accumulator address:

* eccUSDC as an ECC currency with ID `3`;
* `0.1 VMSHELL` (`100000000` base units) to execute the message;
* an empty `payload`.

The purchased SHELL will be sent back to the address of the contract from which the Accumulator received the message.

The current Accumulator address is:

```
0:3535353535353535353535353535353535353535353535353535353535353535
```

This is the legacy address format that must be used in ABI fields such as `dest`, `buyer`, `recipient`, and other message parameters.

In `tvm-cli` 3.x commands where the address is the target of the command itself (`--addr`, `account`, `runx`, or `callx`), Mainnet/Shellnet requires the full `dapp_id::account_id` format. The full CLI address of the Accumulator is:

```
0000000000000000000000000000000000000000000000000000000000000001::3535353535353535353535353535353535353535353535353535353535353535
```

## Exchange Rate and Base Units

The exchange rate is fixed:

```
1 eccUSDC = 100 SHELL
```

Message amounts are specified in base units:

* `1 eccUSDC = 1,000,000` eccUSDC units;
* `1 SHELL = 1,000,000,000` SHELL units.

The Accumulator accepts only whole USDC amounts. The eccUSDC amount must be greater than zero and divisible by `1,000,000`.

|        Sent | Value in `cc["3"]` |     Received |
| ----------: | -----------------: | -----------: |
|   1 eccUSDC |          `1000000` |    100 SHELL |
|  10 eccUSDC |         `10000000` |  1,000 SHELL |
| 100 eccUSDC |        `100000000` | 10,000 SHELL |

Use this formula when preparing the message:

```
cc["3"] = eccUSDC amount × 1,000,000
```

## Before You Exchange

{% stepper %}
{% step %}
### Confirm the wallet has eccUSDC

The wallet has enough eccUSDC with ID `3`.
{% endstep %}

{% step %}
### Confirm the wallet has VMSHELL

The wallet has enough VMSHELL to send the message and pay the transaction fee.
{% endstep %}

{% step %}
### Confirm the destination

The destination address matches the Accumulator address.
{% endstep %}

{% step %}
### Include only eccUSDC

The message contains only one ECC currency: eccUSDC with ID `3`.
{% endstep %}

{% step %}
### Use a whole-eccUSDC amount

The eccUSDC amount is divisible by `1,000,000`.
{% endstep %}

{% step %}
### Set the required value and flags

The `value` field is `100000000` and the `flags` field is `0`.
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
Do not send eccUSDC together with other ECC currencies in the same message. The Accumulator rejects messages containing multiple currencies.
{% endhint %}

## Message Parameters

Use these parameters to purchase SHELL for `1 USDC`:

```json
{
  "dest": "0:3535353535353535353535353535353535353535353535353535353535353535",
  "value": "100000000",
  "cc": {
    "3": "1000000"
  },
  "bounce": true,
  "flags": 0,
  "payload": ""
}
```

| Field     | Value               | Purpose                                               |
| --------- | ------------------- | ----------------------------------------------------- |
| `dest`    | Accumulator address | Contract that performs the exchange                   |
| `value`   | `100000000`         | 0.1 VMSHELL for executing the internal message        |
| `cc`      | `{"3":"1000000"}`   | 1 eccUSDC in base units                               |
| `bounce`  | `true`              | Returns the message to the sender if execution fails  |
| `flags`   | `0`                 | Deducts the forwarding fee from the specified `value` |
| `payload` | Empty string        | Invokes the Accumulator's `receive()` handler         |

{% hint style="info" %}
Use the `value = 100000000` and `flags = 0`. After the fee is deducted, the Accumulator receives approximately `0.098731 VMSHELL`, which is enough to execute the exchange. Do not set `value = 0`: no outgoing message will be created, and the fee for the failed attempt will still be charged.
{% endhint %}

Some wallets may name the ECC currency field `ecc` or `currencies` instead of `cc`. The meaning is the same: key `3` is the eccUSDC identifier, and its value is the amount in base units.

## Send from an UpdateCustodianMultisigWallet\_v2 Multisig

For a wallet with one custodian, call the `sendTransaction` method.

The following Shellnet example exchanges `1 USDC`:

{% hint style="info" %}
For the Mainnet network, use the `mainnet.ackinacki.org` endpoint.
{% endhint %}

```bash
tvm-cli -j -u shellnet.ackinacki.org callx \
  --addr <WALLET_DAPP_ID>::<WALLET_ACCOUNT_ID> \
  --abi contracts/0.81.0_compiled/updatecustodianmultisigwallet_v2/UpdateCustodianMultisigWallet_v2.abi.json \
  --keys <WALLET_KEYS_FILE> \
  --method sendTransaction \
  '{
    "dest":"0:3535353535353535353535353535353535353535353535353535353535353535",
    "value":"100000000",
    "cc":{"3":"1000000"},
    "bounce":true,
    "flags":0,
    "payload":"",
    "dapp_id":"0x0000000000000000000000000000000000000000000000000000000000000001"
  }'
```

The `dapp_id` field in this wallet version is used for transfer reporting. The Accumulator DApp ID is `1`.

{% hint style="warning" %}
Do not use the legacy `0:<WALLET_ACCOUNT_ID>` address in the `--addr` parameter. The current `tvm-cli` accepts only the full `<WALLET_DAPP_ID>::<WALLET_ACCOUNT_ID>` format here. The `dest` value inside the JSON must remain in the legacy `0:3535...` format.
{% endhint %}

If the wallet has multiple custodians, use `submitTransaction` with the same parameters instead of `sendTransaction`, and then collect the required number of confirmations through the wallet's usual process.

{% hint style="warning" %}
Do not use outdated multisig versions.
{% endhint %}

## Receive SHELL at Another Address

With an empty `payload`, the SHELL recipient is `msg.sender`. If a multisig contract sends the message, the SHELL is credited to that multisig contract's balance.

To send the purchased SHELL to another address, call the following Accumulator method:

```
buyShellFor(address buyer)
```

First, generate the ABI-encoded call body:

```bash
tvm-cli -j body \
  --abi contracts/0.79.3_compiled/accumulator/ShellAccumulatorRootUSDC.abi.json \
  buyShellFor \
  '{"buyer":"0:<RECIPIENT_ACCOUNT_ID>"}'
```

Pass the resulting `Message` value in the transaction's `payload` field instead of an empty string. The message must still include eccUSDC with ID `3` and `0.1 VMSHELL` for execution.

Example wallet parameters:

```json
{
  "dest": "0:3535353535353535353535353535353535353535353535353535353535353535",
  "value": "100000000",
  "cc": {
    "3": "1000000"
  },
  "bounce": true,
  "flags": 0,
  "payload": "<ENCODED_BUY_SHELL_FOR_MESSAGE>",
  "dapp_id": "0x0000000000000000000000000000000000000000000000000000000000000001"
}
```

The `buyer` address determines the SHELL recipient. In this case, the SHELL is not returned to the sender.

## Verify the Result

{% stepper %}
{% step %}
### Confirm the eccUSDC balance decreased

The sender's eccUSDC balance with ID `3` decreases by the amount sent.
{% endstep %}

{% step %}
### Confirm the SHELL balance increased

The recipient's SHELL balance with ID `2` increases according to the exchange rate.
{% endstep %}

{% step %}
### Check the event

The Accumulator emits a `ShellPurchased` event.
{% endstep %}
{% endstepper %}

The event contains:

* `buyer`: the SHELL recipient address;
* `usdcAmount`: the amount of eccUSDC received;
* `shellFromSellers`: SHELL obtained from existing seller orders;
* `shellMinted`: additional SHELL minted by the system.

The source of the SHELL does not change the result for the user: `shellFromSellers + shellMinted` is sent to the recipient in a single transaction.

Check the wallet balance with:

{% hint style="info" %}
For the Mainnet network, use the `mainnet.ackinacki.org` endpoint.
{% endhint %}

```bash
tvm-cli -j -u shellnet.ackinacki.org account \
  <WALLET_DAPP_ID>::<WALLET_ACCOUNT_ID>
```

In the `ecc_balance` field:

* key `3` contains the eccUSDC balance in micro-USDC;
* key `2` contains the SHELL balance in nanoSHELL.

After exchanging `1 USDC`, the balance under key `3` should decrease by `1000000`, and the balance under key `2` should increase by `100000000000`.

## Common Errors

<details>

<summary>Amount Is Not a Whole USDC Value</summary>

A `cc["3"]` value such as `1500000` represents `1.5 USDC` and will be rejected. Use only whole amounts such as `1000000`, `2000000`, or `10000000`.

</details>

<details>

<summary>Incorrect ECC ID</summary>

Use ID `3` to purchase SHELL. ID `2` represents SHELL and starts a different operation: creating a SHELL sell order.

</details>

<details>

<summary>Multiple ECC Currencies Included</summary>

The `cc` object must contain only one entry with key `3`. A message containing multiple ECC currencies will be rejected.

</details>

<details>

<summary>SHELL Was Credited to the Multisig Instead of a Personal Address</summary>

This is expected with an empty `payload`: the recipient is the contract that sent the message. Use `buyShellFor` if the SHELL must be sent to another address.

</details>

<details>

<summary>Standard TIP-3 USDC Was Used for the Exchange</summary>

The Accumulator accepts eccUSDC, the ECC currency with ID `3`. Standard TIP-3 USDC must first be converted to eccUSDC through the appropriate bridge.

</details>

## Important

{% hint style="warning" %}
Before sending a large amount, perform a test exchange of `1 USDC` and confirm that the SHELL arrived at the expected address. Blockchain transactions are irreversible.
{% endhint %}
