# Wallets

In the Acki Nacki blockchain, every account is a smart contract. This means a wallet is not a separate entity — its behavior, logic, and capabilities are entirely defined by the type of deployed contract. Different use cases call for different wallet types, from everyday transactions to multi-party fund management.

### How Wallets Work in Acki Nacki <a href="#how-wallets-work-in-acki-nacki" id="how-wallets-work-in-acki-nacki"></a>

Unlike many blockchains where an account is simply a key pair with a balance, in Acki Nacki an Account is a record in a distributed database containing a balance, code, and data. The contract type determines what operations are available to the owner: sending tokens, multisig approval, access recovery, spending limits, and more.Thanks to this architecture, Acki Nacki natively supports account abstraction — every wallet can have its own unique logic defined by its smart contract.

### Wallet Types <a href="#wallet-types" id="wallet-types"></a>

#### Acki Nacki Wallet (Main Network Wallet) <a href="#acki-nacki-wallet-main-network-wallet" id="acki-nacki-wallet-main-network-wallet"></a>

The primary wallet of the Acki Nacki network is a non-custodial smart contract wallet with advanced security features. It is available as a mobile and web application.

**Key features:**

* **ZK Login** — authentication based on zero-knowledge proofs (zk-SNARKs, Groth16). Allows you to sign in using Google, Facebook, and other OpenID providers without exposing private data to the blockchain.
* **Multi-factor protection** — the wallet supports multiple transaction confirmation factors.
* **Access recovery** — ability to recover wallet access in case of device loss or OpenID credential loss.
* **Non-custodial** — no user data is sent to any server; the owner retains full control over their assets.
* **Formal verification** — the wallet code undergoes formal verification for maximum security assurance.

**Install:** [ackinacki.com/wallet​​](https://ackinacki.com/wallet)

***

#### Multisig Wallet <a href="#multisig-wallet" id="multisig-wallet"></a>

If you need a custodial solution or multi-party fund management, you can deploy a Multisig Wallet. This is a smart contract that requires transaction confirmations from a specified number of owners (custodians).

**Key features:**

* **Multiple owners** — the wallet can have one or more owners, each with their own key.
* **Confirmation threshold** — the number of required signatures to execute a transaction is configured at deployment (`reqConfirms`).
* **Flexibility** — suitable for both personal use (single owner) and team or corporate scenarios.
* **Deployment via TVM CLI** — the wallet is deployed using the `tvm-cli` command-line tool.

**Deployment guide:** [dev.ackinacki.com/how-to-deploy-a-multisig-wallet​​](https://dev.ackinacki.com/how-to-deploy-a-multisig-wallet)

***

