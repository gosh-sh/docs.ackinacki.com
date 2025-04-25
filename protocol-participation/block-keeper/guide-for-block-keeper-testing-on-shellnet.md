---
description: Test joining the protocol
---

# Guide for Block Keeper testing on Shellnet

**1. Obtaining the License Owner's Key Pair**

Go to the [Acki Nacki staging dashboard](https://dashboard-dev.ackinacki.com/).

Follow the instructions in the [License Dashboard Guide](https://docs.ackinacki.com/protocol-participation/license/license-dashboard-guide) to obtain the License Owner's key pair.

{% hint style="info" %}
Save the generated key pair in the `License.keys.json` file. You will need it later to manage the License contracts.
{% endhint %}

**2. Gaining Access to the User's License Contracts**

The License Owner should send their **`public key`** to a GOSH representative via [Telegram](https://t.me/Oxydixi).

The GOSH team will deploy licenses and provide you with the their addresses and unique on-chain numbers.

**5. Deploying a Block Keeper Wallet with a Whitelist**

{% hint style="warning" %}
Before deploying a BK Wallet all the delegating licenses have to be deployed, \
and their on-chain unique numbers have to be collected. \
They will be placed into the wallet's whitelist during deployment.
{% endhint %}

To deploy the BK wallet, follow the instructions in the [Deployment Guide](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#block-keeper-wallet-deployment).&#x20;

As a result, you will get something like the following:

```

File ../../../../bk_wallet/master.keys.json not found. Generating master keys...
Deploying wallet...
...
Wallet 0:dac2f1bf07a23e8c85393022de358a017d605ebe930d6dad6fd4777ce5a4c09b is deployed.
License number 6 and license address is 0:7f2f945faaae4cce286299afe74dac9460893dd5cba1ac273b9e91f55f1141ec
License number 7 and license address is 0:deffa917f23c45afb263fadbb08406c2291447017f79f480417ca4de5551d1e2
Current minimum stake is 1 NACKLs
Sending 1 NACKLs
...
Checking wallet balance...
Current wallet balance: 1
Node ID: dac2f1bf07a23e8c85393022de358a017d605ebe930d6dad6fd4777ce5a4c09b
Initial steps have been done. Save your node id
```

{% hint style="warning" %}
Save your BK wallet address, `Node ID`, and the file containing the BK Node Owner keys — these will be needed to run the staking script.
{% endhint %}

**5. Delegating a License to a BK Wallet**

{% hint style="warning" %}
A maximum of 10 licenses can be delegated to a single node.
{% endhint %}

To delegate a license, a License Owner should call the `addBKWallet(uint256 pubkey)` method in a [License contract](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.sol) in each License contract.

Where:

`pubkey` - the BK Node Owner's pubkey.

Example of calling the method:

```

tvm-cli -j callx --addr 0:7f2f945faaae4cce286299afe74dac9460893dd5cba1ac273b9e91f55f1141ec --abi acki-nacki/contracts/bksystem/License.abi.json --keys license_owner_keys/license.keys.json --method addBKWallet '{"pubkey": "0xfa4edc8b63c4e66241a57c11e0a522769ca4a4f106692512fc92f2d658169bcc"}'
```

**You can check the delegation status by calling the `getBK()` method in the License contract.**

Example command:

```

tvm-cli -j runx --addr 0:7f2f945faaae4cce286299afe74dac9460893dd5cba1ac273b9e91f55f1141ec --abi acki-nacki/contracts/bksystem/License.abi.json --method getBK
```

The method will return the BK wallet address to which the license has been delegated:

```
{
  "bkwallet": "0:dac2f1bf07a23e8c85393022de358a017d605ebe930d6dad6fd4777ce5a4c09b"
}
```

or `null` if no delegation has occurred:

```
{
  "bkwallet": null
}
```

You can obtain the on-chain unique number of the License by calling the `getDetails()` method in the License contract:

```
tvm-cli -j runx --addr 0:deffa917f23c45afb263fadbb08406c2291447017f79f480417ca4de5551d1e2 --abi acki-nacki/contracts/bksystem/License.abi.json --method getDetails
```

result:

```
{
    "license_number": "0x0000000000000000000000000000000000000000000000000000000000000007",
    "bkwallet": "0:dac2f1bf07a23e8c85393022de358a017d605ebe930d6dad6fd4777ce5a4c09b",
    "owner_pubkey": "0x90f19f961898d394f68151a756140942767a91e354607f24150d180de1a085d7",
    "owner_address": null,
    "reputationTime": "0"
}
```

**Learn more about** [**how to work with licenses**](../license/working-with-licenses.md)**.**

**6. Staking**

Only after completing all previous steps can the Node Owner run the [**staking script**](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#staking)**.**

{% hint style="warning" %}
To initiate staking, at least one license must be delegated to the node.
{% endhint %}
