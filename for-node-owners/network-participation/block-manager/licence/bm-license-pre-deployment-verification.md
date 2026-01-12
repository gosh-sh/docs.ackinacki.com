# BM License Pre-Deployment Verification

Веfore the BM License contract is deployed to the **Acki Nacki** network, the **BM License Owner** **must launch the Block Manager (BM) service** **and provide it for functionality auditing by a GOSH representative**

For this, it’s necessary to:

### Preparing the BM Service

1. The BM License owner must agree on cooperation with the owner of one of the BK nodes, who must:

* Open a port for connection to the block streaming service.
* Provide the following:
  * **BK node IP address:** `NODE_IP`&#x20;
  * **API access token** to retrieve accounts: `BK_API_TOKEN`&#x20;

{% hint style="warning" %}
Only **one Block Manager service** can be registered per **NODE\_IP**
{% endhint %}

2. Generate two key pairs:

* One key pair will be used to operate the **BM wallet**.
* The other will be used to **sign external message authentication tokens**.

```
tvm-cli genphrase --dump block_manager.keys.json
tvm-cli genphrase --dump block_manager_signing.keys.json
```

3. Deploy the BM wallet using the following command:

```
tvm-cli --abi ../contracts/bksystem/BlockManagerContractRoot.abi.json --addr 0:6666666666666666666666666666666666666666666666666666666666666666 -m deployAckiNackiBlockManagerNodeWallet '{"pubkey": "0xBLOCK_MANAGER_PUB_KEY", "signerPubkey": "0xBLOCK_MANAGER_SINING_PUB_KEY", "whiteListLicense": {}}'
```

{% hint style="warning" %}
At this stage, the `whiteListLicense` field must remain **empty**.
{% endhint %}

4. Prepare an **Ansible inventory file** for BM deployment, specifying all required variables.\
   [An example inventory file can be found here](https://github.com/ackinacki/ackinacki?tab=readme-ov-file#create-an-ansible-inventory)

### Verification and Functionality Confirmation

After successfully launching the BM service, make sure to:

* Verify that the BM is correctly connected to the blockchain and is receiving block streams.
* Provide the IP address of your BM (`HOST_PUBLIC_IP`) for verification.&#x20;

#### 3. License Activation

After the service is verified:

* The **BM License contract** will be deployed to the network.
* The **contract address `LICENSE_ADDR`** and your **License number** will be received from a GOSH representative.

The BM License Owner must then update the whitelist in the BM wallet by adding their License number, thereby granting permission for delegation:

````

```bash
tvm-cli -j callx --addr BM_WALLET_ADDR --abi contracts/bksystem/AckiNackiBlockManagerNodeWallet.abi.json --keys block_manager.keys.json --method setLicenseWhiteList '{"whiteListLicense": {"YOUR_LICENSE_NUMBER": true}}'
```
````

* then delegated the License to the wallet:

```

tvm-cli -j callx --addr LICENSE_ADDR --abi contracts/bksystem/LicenseBM.abi.json --keys BM_LICENSE_OWNER.keys.json --method addBMWallet '{"pubkey": "0xBLOCK_MANAGER_PUB_KEY"}'
```

