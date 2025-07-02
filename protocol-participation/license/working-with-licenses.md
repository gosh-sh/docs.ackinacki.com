---
description: The described flow will be applicable after the network is launched.
---

# Working with Licenses

<figure><img src="../../.gitbook/assets/DL (1).png" alt="" width="563"><figcaption></figcaption></figure>

## Obtaining License Owner Keys and License Contract Addresses

The License Owner [registers](https://docs.ackinacki.com/protocol-participation/license/license-dashboard-guide) in the [dashboard](https://dashboard.ackinacki.com) using a third-party crypto wallet \[1.1] that was used to purchase the license. Upon registration, the License Owner will receive a generated seed phrase and a corresponding key pair \[1.2].

{% hint style="warning" %}
To manage their license contacts, it is recommended that the License Owner save the obtained keys in a file named `License.keys.json`.&#x20;
{% endhint %}

The **public** key from this pair must be sent to a GOSH representative via email \[1.3]. This key will be used by the [`LicenseRoot`](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/LicenseRoot.sol) system contract \[1.4] to deploy the [`License`](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.sol) contract \[1.5].

{% hint style="info" %}
A separate License contract is deployed for each license purchased.&#x20;
{% endhint %}

Once the License contract is deployed, the GOSH representative will provide the License Owner with the contract address and [License number](../../glossary.md#license-number)  \[1.7].

## Adding a License to a BK Wallet Whitelist

For the license to be delegated, its license number must be added to the [BK wallet whitelist](../../glossary.md#bk-wallet-whitelist).

To do this, the License Owner must provide their License Number \[2.1] to the Node Owner, who will then add it to their whitelist \[2.2].

{% hint style="warning" %}
There is no limit to the number of licenses that can be added to a BK Wallet whitelist; \
however, only **20 (twenty)** of them can be delegated.
{% endhint %}

## BK Wallet Deployment

The BK wallet [is deployed by the Node Owner](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#block-keeper-wallet-deployment) using the [deployment script](https://github.com/ackinacki/ackinacki/blob/main/scripts/create_block_keeper_wallet.sh) \[3].

{% hint style="info" %}
The wallet can be deployed with an empty whitelist or pre-filled with any number of licenses the Node Owner chooses to authorize. But only **20 (twenty)** licenses of these can be delegated later.
{% endhint %}

{% hint style="info" %}
The Node Owner can modify the whitelist at any time by using the setLicenseWhiteList(mapping(uint256 => bool) whiteListLicense) method in the [`AckiNackiBlockKeeperNodeWallet`](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/AckiNackiBlockKeeperNodeWallet.sol) contract. \
Pass in the license numbers you received from the License Owners.

* **`uint256` (key)** – the license number.
* **`bool` (value)** – set to `true` to **add** the license on the whitelist, or `false` to **remove** it.\


Example command:

{% code overflow="wrap" %}
```sh
tvm-cli call BK_WALLET_ADDR setLicenseWhiteList '{"whiteListLicense": {"1": true, "2": true, "5": true}}' --abi acki-nacki/contracts/bksystem/AckiNackiBlockKeeperNodeWallet.abi.json --sign BK_NODE_OWNER_KEYS
```
{% endcode %}

\
You will need the ABI file [AckiNackiBlockKeeperNodeWallet.abi.json](https://raw.githubusercontent.com/gosh-sh/acki-nacki/refs/heads/refactor/fee/contracts/bksystem/AckiNackiBlockKeeperNodeWallet.sol?token=GHSAT0AAAAAADCMNSQIMA3ZIFGSUAMDQGQY2DDV64Q) to run this command.
{% endhint %}

## Providing the BK Node Owner Public Key

To initiate delegation, the Node Owner must send their public key (`BK_NODE_OWNER_PUBKEY`) to the License Owner \[4].

## Delegating a License

{% hint style="warning" %}
A maximum of **20 (twenty)** licenses can be delegated to a single node.
{% endhint %}

To delegate a license to a specific node, the License Owner must call the `addBKWallet(uint256 pubkey)` method on their [`License`](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.sol) contract \[5.1]:

```

tvm-cli call <LICENSE_ADDRESS> addBKWallet '{"pubkey": "0x<BK_NODE_OWNER_PUBKEY>"}' --abi License.abi.json --sign License.keys.json
```

Where:

* `pubkey` is the public key of the Node Owner received in step 4,
* [`License.abi.json`](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/License.abi.json) is the ABI of the License contract,
* `License.keys.json` is the keys obtained by the License Owner during registration in the dashboard.

The License contract will send a request to the [BK wallet contract](https://github.com/ackinacki/ackinacki/blob/main/contracts/bksystem/AckiNackiBlockKeeperNodeWallet.sol) \[5.2], which will verify whether the License is included in its BK wallet whitelist.\
If the License is whitelisted, the BK wallet accepts the delegation. \
Otherwise, the delegation request is denied.

To check the current delegation status, the `getBK()` method can be called on the `License` contract:

```

tvm-cli -j runx --addr <LICENSE_ADDRESS> --abi acki-nacki/contracts/bksystem/License.abi.json --method getBK
```

It will return the address of the BK wallet to which the license has been delegated, \
or `NULL` if no delegation has taken place.

## Staking

Only after successful delegation of at least 1 license can the Node Owner proceed to run the [staking script](https://github.com/ackinacki/ackinacki/tree/main?tab=readme-ov-file#staking) \[6].

{% hint style="warning" %}
At least one license must be delegated to the node in order to initiate staking.
{% endhint %}
