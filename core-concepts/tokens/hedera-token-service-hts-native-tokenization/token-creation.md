# Token Creation

## Overview

Creating tokens with the Hedera Token Service (HTS) is a streamlined process designed to help developers issue both fungible and non-fungible tokens quickly and securely on the Hedera network. Whether you’re launching digital currencies, loyalty points, or unique digital collectibles, HTS offers a robust framework for token creation and management. During the token creation process, you can customize properties like the token name, symbol, supply details, and administrative keys. Once the transaction is processed, a unique token ID is generated, which serves as the reference for all future token operations.

***

## HTS Token Creation Flow

### 1. Define Token Properties

Before initiating the token creation process, clearly define the token’s properties. Depending on the type of token you're creating, the required properties may vary. See examples below.

<details>

<summary><strong>Fungible Token Properties</strong></summary>

**Key Properties:**

* **Token Details:**
  * **Name & Symbol:** Human-readable identifiers.
  * **Decimals:** Defines token divisibility.
  * **Initial & Maximum Supply:** Determines the token economy.
* **Key Roles:**
  * **Admin Key:** Governs administrative changes.
  * **Supply Key:** Controls minting and burning operations.
  * **Freeze, KYC, & Pause Keys:** Enhance security and regulatory compliance.
* **Treasury Account:**
  * Holds the initial supply and manages distribution.

</details>

<details>

<summary><strong>Non-Fungible Token Properties</strong></summary>

**Key Properties:**

* **Token Details:**
  * **Name & Symbol:** As with FTs, but often without a decimals property.
  * **Unique Metadata:**
    * Each token holds unique identifiers (serial numbers) and metadata that distinguishes it from others.
* **Key Roles & Treasury:**
  * Similar key roles apply, but NFT-specific operations may involve unique signing procedures for each minting event.

</details>

{% hint style="info" %}
_**➡** The full list of token properties can be found on the_ [_Token Properties_](token-properties.md) _page._
{% endhint %}

### 2. Create the Token Creation Transaction

Use the HTS API to create a token creation transaction that incorporates all the defined properties. Ensure that you:

* Specify the treasury account.
* Include the necessary administrative keys (such as the admin key).
* Confirm that the initiating account has sufficient funds (in tinybars) to cover the transaction fee.

### 3. Sign and Submit the Transaction

Use the required treasury or admin keys to sign the transaction securely before submitting the signed transaction to the Hedera network. The network processes the transaction and, upon successful execution, generates a unique token ID.

### 4. Confirm Token Creation and Get the Token ID

Once the network confirms the transaction, get the transaction receipt, which includes the new token ID generated from the previous step. Use queries such as the `TokenInfoQuery` to confirm the token’s properties and ensure that all configurations are correctly set.

<figure><img src="../../../.gitbook/assets/token-creation-flow.png" alt="" width="563"><figcaption></figcaption></figure>

***

## Muteable vs. Immutable Tokens

* **Mutable Tokens:**\
  These tokens can be updated after creation (e.g., changes to supply or metadata) and require keys that are set during creation by designating keys (such as the admin key) that authorize changes.
* **Immutable Tokens:**\
  Once created, these tokens cannot be changed. No administrative keys are assigned, ensuring that the token’s properties remain fixed by omitting or restricting the relevant keys (e.g., by not assigning an admin key) to prevent modifications.

***

## Transaction Signing Requirements

Token operations require specific keys to ensure that only authorized actions are executed:

<table><thead><tr><th width="179">Key</th><th>Description</th></tr></thead><tbody><tr><td><strong>Admin Key</strong></td><td>Grants full control over token configuration, including updating properties. Used for governance and administrative tasks, such as freezing or pausing.</td></tr><tr><td><strong>Supply Key</strong></td><td>Allows minting and burning of tokens to adjust the total supply. Ensures control over token issuance and circulation.</td></tr><tr><td><strong>Freeze Key</strong></td><td>Enables freezing or unfreezing token transfers for specific accounts. Used to enforce restrictions during investigations or compliance checks.</td></tr><tr><td><strong>KYC Key</strong></td><td>Enforces KYC compliance, allowing only approved accounts to transact with tokens. Ensures regulatory compliance for sensitive use cases.</td></tr><tr><td><strong>Wipe Key</strong></td><td>Allows removal of tokens from specific accounts. Used for refunding or correcting token allocations.</td></tr><tr><td><strong>Pause Key</strong></td><td>Allows pausing or unpausing token transactions. Pausing a token prevents the token from participating in all transactions.</td></tr><tr><td><strong>Fee Schedule Key</strong></td><td>Allows adjustments to the token’s custom fee schedule, providing flexibility in managing transaction costs.</td></tr><tr><td><strong>Metadata Key</strong></td><td>The key which can update the metadata of an NFT. This key is used to sign and authorize the transaction to update the metadata of NFTs. This value can be null.</td></tr></tbody></table>

***

## Consensus Nodes vs. Mirror Nodes in the Hedera Stack

On the Hedera network, the consensus nodes and mirror nodes perform two distinct operations at various layers of the Hedera stack.&#x20;

<figure><img src="../../../.gitbook/assets/consensus-transaction-ordering-process.webp" alt=""><figcaption><p>Consensus transaction ordering process</p></figcaption></figure>



**Consensus nodes** are responsible for executing all state-changing transactions, including HTS token creation, minting, burning, and transfers. They validate and order transactions, ensuring finality and security. Once transactions are processed, they generate transaction receipts containing details like the token ID.

**Mirror nodes** provide a data layer for querying real-time and historical ledger information without impacting network performance. Developers can use them to fetch token details, track NFT ownership history, and view token transaction logs. By leveraging mirror nodes, developers can access detailed insights into their token’s history and state without requiring direct interaction with consensus nodes.
