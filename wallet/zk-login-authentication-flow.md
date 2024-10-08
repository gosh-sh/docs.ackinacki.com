# ZK Login Authentication Flow

## Overview

Aсki Naсki is a blockchain designed for the convenience of users. Authentication in Wallet smart-contract is one of the most important aspects of using it. In Aсki Naсki we use some special authentication scheme, which allows users to combine convenience and security. It provides the ability to send transactions using an OAuth credential, without publicly linking blockchain accounts and social network accounts.&#x20;

So we offer transaction authentication via social network accounts working through OAuth 2.0 (more particularly OpenID) protocol like Google, GitHub etc, which allows for quick authentication without burdening the user with remembering seed phrase in majority of cases. But we still use seed phrase at least for recovering purposes. And user may choose to authenticate transactions for huge values using seed phrase also.

In general our scheme was inspired by [zkLogin](https://docs.sui.io/concepts/cryptography/zklogin),  but we decided to improve security by replacing the server-generated salt with a user-owned Password.

Additionally we add the ability to recover access to wallet in accidental cases (for example the case of device loosing).

As a result we obtain a multi-factor authentication scheme that allows users to not enter any additional information to send everyday transactions while the JWT token has not expired. At the same time attackers who compromise User's social network account cannot transact unless they separately compromise the user-owned Password.

And even if the user loses the device and Password, he/she will be able to restore access and at the same time prevent the adversary from using his/her wallet.

**Desired Properties that we provide:**

* In the majority of cases we are able to transact on Acki Nacki using the familiar OAuth authentication flow. However we do not eliminate the necessity in mnemonics to provide ability to recover access to Wallet and extra security guarantees for huge transactions.
* Transaction requires approval from the user via the standard OAuth login process, but OpenID provider (or attacker who compromises OAuth account) cannot transact himself, pretending to be the user. This is provided via an extra Password.
* zk-proofs prevent third parties from linking Acki Nacki blockchain address with its corresponding account in social network.

{% hint style="info" %}
We support the following list of OpenID providers: Google, Github, Twitter, Facebook, AWS.
{% endhint %}



## **Seed Phrase, Password and Recovery Password**

To initialize Wallet you must provide social network account data (for example Google email). You should remember account related data and standard passkey for your smartphone to authenticate. Also there are some secrets that you need to handle in order to be able to access your Wallet contract. These are your Password, Recovery Password and your Seed Phrase.&#x20;

You create the Password yourself, so that it is convenient for you to remember and use it. You also create the Recovery Password by yourself, but it must meet certain security conditions (be long enough and contain both digits and characters). Seed Phrase is generated on your device. All this stuff is done just before Wallet smart-contract deploying and it is used for  deployment. &#x20;

{% hint style="danger" %}
You should store **Password, Recovery Password,** and your **Seed Phrase** in a secret place.&#x20;
{% endhint %}

During Wallet smart-contract deployment Seed Phrase and Recovery Password are used by Mobile device to derive keypairs (SK\_SeedPhrase, PK\_SeedPhrase) and (SK\_Recovery, PK\_Recovery).&#x20;

Also there is a hash zkID generated using social network account data (email) and  user's Password.  It is used to link Wallet smart-contract and social network account, but anonymously. We don't want to expose email/social account related to a particular Wallet in blockchain. So Wallet contract stores triple: zkID, PK\_SeedPhrase, PK\_Recovery as shown in the picture below.

{% hint style="info" %}
To make it more difficult for a potential attacker to brute force passwords, we use PBKDF Argon-2 to generate zkID from a (possibly weak) user's Password (later more details about zkID generation). Hash function used  to calculate zkID  is the standard collision resistant Poseidon function.
{% endhint %}

{% hint style="info" %}
The Recovery Password may be stored in some safe place, for example SGX enclave on server or it can be replaced with a security card storing the respective key pair.
{% endhint %}

{% hint style="info" %}
Our Client App does not backup  Password, Recover Password, Seed Phrase and related secret keys. It will store only fresh JWT token related to social network account authentication and some extra stuff like zk-proof that we will discuss below.
{% endhint %}

{% hint style="info" %}
The Issuer on the diagram below is a type of social network provider (Google, GitHub etc).
{% endhint %}

<figure><img src="../.gitbook/assets/The contents of the Wallet contract.jpg" alt=""><figcaption></figcaption></figure>

## Authentication sketch

Here we briefly review the essence of the idea to transact using OpenID. In further sections there is more detailed description.

* A **JWT**  is a signed token from OpenID provider, containing a payload that includes a field named 'nonce'. We add into nonce such user data as ephemeral public key and date/time of its expiration.
* The **wallet (client app)** generates and stores an ephemeral key pair, where the ephemeral public key is added into nonce. The ephemeral private key signs transactions during some short period of time, eliminating the need for the user to backup it.
* The Groth16 zero-knowledge proof is generated based on JWT. The aim of generating the proof is to prove that the user really owns the account, i.e. has related signed valid JWT. However JWT contains fields that deanonymize users. That's why we generate proof per JWT to hide some JWT fields.
* A transaction is submitted on-chain being signed by an ephemeral secret key and supplied with zero-knowledge proof. TVM executes the transaction after verifying the ephemeral signature and the zk-proof.\


## Main Entities

**Application frontend -** this is Client frontend application that supports our flow to create and authenticate transactions. Client frontend application is responsible for deploying Wallet smart-contract with valid user's data, storing the ephemeral private key, directing users to complete the OAuth login flow, creating and signing transactions.

**Proof Service -** this is a backend service responsible for generating zero-knowledge proofs based on JWT, extra randomness, user Password and max epoch. This proof is submitted on-chain along with the ephemeral signature for transaction.



## Keys priorities and loss case handling

**Master key pair (SK\_SeedPhrase, PK\_SeedPhrase)** – that is used to maintain Wallet smart-contract. Knowledge of **SK\_SeedPhrase (Seed Phrase)** allows you to change zkID or **PK\_RecoveryPassword** in contract.&#x20;

{% hint style="warning" %}
To change PK\_SeedPhrase in contract one should have: access to OpenID account, Password, Recovery Password.
{% endhint %}

We have a protection against several of the most likely accident scenarios of loss.

* If lost Mobile Device and/or access to social network account and/or Password, then use Seed Phrase to change zkID in Wallet smart-contract.
* If you lost RecoveryPassword, then again use Seed Phrase to change  SK\_RecoveryPassword by fresh  SK’\_RecoveryPassword in contract.
* If lost Seed Phrase, then User must have phone with Not yet Expired JWT and respective related extra data like zk-proof + Recovery Password (i.e.  PK\_RecoveryPassword). It allows you to change PK\_SeedPhrase in contract.
* If lost Seed Phrase and lost  phone/or JWT is expired, then to change PK\_SeedPhrase User needs social network account access, Password and Recovery Password (i.e.  PK\_RecoveryPassword).

**Remark.** For now it is the responsibility of User to make a strong Recovery Password and backup it. The ideal Recovery Password is the second Seed Phrase, but this is too cumbersome. \[So let’s think about some system of security questions and answers?]

**Remark.** If we use an enclave or security card for recovery, then SK\_RecoveryPassword does not leave outside the enclave (like for security cards) and it just signs the User's message after the User provided some Recovery Password. Recovery Password may be weak (even simple extra PIN), but enclave controls the number of attempts (like in Signal).

**Remark.** (SK\_RecoveryPassword, PK\_RecoveryPassword) is a key pair that is used only in the case if  Seed Phrase was lost by the User.



## Detailed description of Wallet transactions maintaining

### Normal operation (transaction) WITH signIn to OpenID provider

At the very beginning User starts with signIn to the relevant social network account supporting OpenID protocol. To make a signIn request User generates ephemeral random temporary key pair $(SK\_e, PK\_e)$. Public key $PK\_e$, the data about its expiration time and extra generated randomness are concatenated and put into  'nonce' field that is added into a semi-finished JWT token prepared by Client application. The last one is sent to the OpenId provider together with other authenticating data. OpenId provider authenticates User. In the case of success, the provider signs JWT with his public fresh JWK RSA key and sends it back.

Signed JWT is a kind of certificate for $PK\_e$ issued by an OpenId provider.

After obtaining a valid JWT user must also input his Password. Together with JWT it will be sent to to Proof service  (see below)

As we want to provide anonymity, we can not send JWT into Wallet contract to prove that User is a valid owner of email embedded into both JWT and zkId previously stored by contract. Instead of this we produce zero knowledge Groth16 proof to prove that User got such JWT. And the contract must verify this proof.

We suppose that the Client application may be run on a smartphone, so the User has small computational power. Groth16 proof calculation is computationally hard, that's why we can not do it in Client application. We deploy our own Proof service for computing proofs. And as we said earlier Client application sends a request to Proof service providing as input JWT and Password discussed above. So more particular private input to calculate zk-proof contains the following data: JWT, Password, ephemeral public key, jwt randomness, key claim name (i.e. sub). The Proof service generates zk-proof that takes these as private inputs and does the following:&#x20;

* checks the nonce is correctly formed;
* checks that key claim values match (like email, i.e stable id, or issuer, aud and others) the corresponding fields in the JWT;
* verifies the RSA signature from OpenID provider on the JWT;
* zkID is consistent with email, issuer and Password.

Groth16 proof computed by Proof service is sent to Wallet contract to authenticate the User. However to prevent the case of malicious Proof service pretending by User we add an extra step into the authentication process. User must sign a message sent into the Wallet contract by $SK\_e$ that only User knows.

All described flow is depicted in the picture below.

**Remark.**  There is a single key pair (proof\_key, verify\_key) generated during the setup phase called Powers-of-tau ceremony, which we describe below. This key pair is generated only once and depends on related to zk-proof circuit that contains formalized JWT verification and other stuff that we will discuss later. The keys from this key pair are NOT secret and used for all clients later. So Proof service uses proof\_key (that is the same for everyone) to generate proof. In the meantime verify\_key is embedded into TVM that has a  respective instruction VERGRTH16 using this key to verify zk-proof. The last instruction is used in the Wallet contract.

The described process is fulfilled by User at the initial moment of time. And then each time when keypair (SK\_e, PK\_e) becomes expired, User must relogin to get fresh JWT for new PK\_e. But until PK\_e is not expired, Client App must store JWT and related zk-proof generated by Proof service and use it for each transaction for User's convenience. The frequency of relogin in OpenId provider is regulated by us. Recall that we specify the time of expiration in nonce of JWT. Contract accepts zk-proof for JWT with nonce into which PK\_e and desired expiration time limit are embedded. Expiration time is verified during zk-proof verification. So we may not depend on OpenId provider standard expiration time for JWT set up by provider itself.

**Remark.** In the case of a transaction asking to transfer an essential amount of money, we can enforce relogin. And also here we may ask for extra signature of transaction done by SK\_SeedPhrase..

**Remark**. Secret key SK\_e is stored in local storage in browser or secure storage/element in a smartphone locked by passkey.

<figure><img src="../.gitbook/assets/zk-Oauth with relogin.jpg" alt=""><figcaption><p>ZK-OAuth (Normal operation with relogin in OpenID provider)</p></figcaption></figure>



### Normal operation (transaction) without signIn to OpenID provider

The next picture demonstrates the process of handling wallet transactions in the case when (SK\_e, PK\_e) and related JWT and zk-proof are present and not expired yet.

<figure><img src="../.gitbook/assets/zk-Oauth without relogin.jpg" alt=""><figcaption><p>ZK-OAuth (Normal operation without relogin in OpenID provider)</p></figcaption></figure>

## More details on handling OAuth 2.0 via Zero-knowledge proofs

As we said before in Acki Nacki blockchain we allow users to login into the wallet with OAuth tokens using JWT tokens from Google, Facebook, Github or other major services for authentication supporting OpenID without revealing these tokens themselves (and therefore without leaking access to the original service and preserving anonimity). This is achieved through a zero-knowledge proof protocol that provides blind verification of the properties of JWT tokens. We use Groth16 (over the elliptic curve BN254), a non-interactive (i.e. one that does not require multiple rounds of interaction between the prover and the verifier) zero-knowledge proof verification system.&#x20;



### Recall OAuth 2.0 and JSON Web Tokens (JWTs)

We use OAuth 2.0 protocol, more particularly OpenID. In this protocol a user can log into a trusted third party (Google, Facebook, etc.) and get a signed access token attesting that they logged in the form of a signed **JSON Web Token (JWT)**. A signed JWT looks like three base64-encoded payloads separated by a dot:

<figure><img src="../.gitbook/assets/A signed JWT.jpg" alt=""><figcaption><p>A signed JWT</p></figcaption></figure>

When decoded, the first part of the payload is a header, the second is the JWT's content itself (called the payload), and the third one is the signature that is done by OpenID provider. One can use the debugger on jwt.io to inspect such JWTs:

<figure><img src="../.gitbook/assets/Using the debugger to inspect JWTs.jpg" alt=""><figcaption><p>Using the debugger to inspect JWTs</p></figcaption></figure>

There are following important fields in the payload of JWT:

* the issuer \`iss\` field, indicates who issued and signed the JWT.
* the audience \`aud\` field, indicates who the JWT was meant for.
* the subject \`sub\` field, represents a unique user ID (from the point of view of the issuer) who the JWT is authenticating.
* the nonce field contains a user nonce for application to prevent replay attacks.

### Verifying JWTs

To verify a JWT, one needs to verify the signature over the JWT. To verify a signature over a JWT, one must know the public key of the issuer of the JWT. Issuers all have a published **JSON Web Key Set (JWKS)**. For example, Facebook's JWKS can be downloaded from https://www.facebook.com/.well-known/oauth/openid/jwks and looks like the picture below.

<figure><img src="../.gitbook/assets/Facebook&#x27;s JWKS.jpg" alt=""><figcaption><p>Facebook's JWKS</p></figcaption></figure>

The JWKS contains several Json Web Keys (JWKs) identified by their key id \`kid\`. Several keys are often displayed to provide support for key rotation. Since this information is external to the JWT, the network must know who the issuer is, and specifically \`kid\` that was used to issue the JWT.

Since the issuer of a JWT is contained in the payload, not in the header, the zk-auth circuit (described below) must extract this value and witness it in its public input.

### The zk-auth circuit

Here we discuss what zk-auth circuit does at a high level. Given the following public input:

* the issuer \`iss\` field (that we expect to find in JWT);
* the RSA public key of the issuer.

It extracts the following as public output:

* the ephemeral public key contained in the \`nonce\` field of the JWT, as well as some expiration information;
* zkID value introduced before which is a hash linking user's account (stable id in social network) with with blockchain address;
* the header of the JWT (which the network needs to validate, and also contains the key ID used by the issuer)
* the audience \`aud\` field of the JWT.

ZK-Auth circuit in addition to extracting above public outputs performs the following:

* It inserts the actual JWT in the zk-circuit as a private witness.
* It checks that the issuer passed as public input is indeed the one contained in the JWT.
* It hashes the JWT with SHA-256 and then verifies the signature (passed as private input) over the obtained digest using the issuer's public key (passed as public input).
* It derives zkID value deterministically using the Poseidon hash function and the user identifier (e.g. an email) as well as some user randomness.

The signature is verified in zk-auth circuit to avoid issuers from being able to track users on-chain via the signatures and digests.

The idea at this point is for the network to make sure that, besides the validity of the zk-proof, the address is strongly correlated to the user. To do that, validators deterministically derive the user's address based on the zkID output by zk-circuit, as well as the issuer and audience fields of the JWT. This information should be enough to uniquely identify the user.

