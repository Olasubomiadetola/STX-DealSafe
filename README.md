
## STX-DealSafe - A Simple Escrow and Trust Protocol for P2P Transactions

TrustBridge is a Clarity smart contract designed to facilitate **secure peer-to-peer (P2P) transactions** via an **escrow system**, while also enabling **reputation tracking** through a trust rating mechanism. It provides atomic, tamper-proof deal creation, payment handling, and mutual rating of participants — all on-chain.

---

### 📜 Overview

The protocol enables two parties (an *initiator* and a *counterparty*) to create a deal involving a STX-denominated payment. Funds are held in escrow until released by the initiator. The counterparty can then leave a **trust rating** for the initiator, contributing to their on-chain reputation.

---

### 🚀 Features

* 🔐 **Escrow Payments**: Funds are held securely until confirmed and released by the sender.
* 🤝 **Deal Management**: Create, view, and rate deals with unique IDs.
* ⭐ **Trust Scores**: Counterparties rate initiators post-transaction to build a trust profile.
* 📑 **Transparent Recordkeeping**: All transactions and ratings are stored on-chain and queryable.

---

### 📂 Contract Structure

#### Constants

* `CONTRACT-OWNER`, `ADMIN`: Role constants (both default to `tx-sender`)
* Error codes: `ERR-NO-AUTH`, `ERR-LOW-VALUE`, `ERR-INVALID-USER`, etc.

#### Data Storage

* `payments`: Maps `id → payment details`
* `deals`: Maps `deal-id → deal metadata`
* `trust-profiles`: Maps `principal → trust score + deal count`
* `payment-id-counter`, `deal-counter`: Auto-increment counters

---

### 🔧 Functions

#### ✅ Public Functions

| Function                             | Description                                                             |
| ------------------------------------ | ----------------------------------------------------------------------- |
| `initiate-deal(counterparty, value)` | Creates a new escrow deal with a specified counterparty and STX amount. |
| `complete-payment(payment-id)`       | Transfers STX to the counterparty and marks payment as complete.        |
| `rate-counterparty(deal-id, rating)` | Allows counterparty to rate the initiator post-deal.                    |
| `get-trust-profile(address)`         | Returns the trust profile (score + deal count) of a user.               |
| `get-payment-info(payment-id)`       | Fetches payment details by ID.                                          |
| `get-deal-info(deal-id)`             | Fetches deal details by ID.                                             |

#### 🔒 Private Functions

| Function                              | Purpose                                   |
| ------------------------------------- | ----------------------------------------- |
| `validate-counterparty(counterparty)` | Ensures counterparty isn't self or admin. |
| `validate-deal-id(deal-id)`           | Checks if deal ID is valid and exists.    |
| `is-valid-user(recipient)`            | Validates that user isn't self or admin.  |

---

### 🔁 Workflow

1. **Initiator creates a deal** with a counterparty and value.
2. Deal details and payment metadata are stored.
3. Initiator **completes payment**, releasing funds.
4. Counterparty **rates the initiator** to update their trust profile.

---

### 🔐 Security Checks

* ❌ Prevents self-deals.
* ⛔ Rejects zero-value transactions.
* ✔️ Only involved parties can perform actions.
* 🕵️ Validates existence of deals and payments before proceeding.

---

### 📊 Trust Score Mechanics

Each time a deal is rated:

* `cumulative-score` is incremented by rating.
* `deal-count` is incremented by 1.
* The current average score = `cumulative-score / deal-count`.

---

### 🧪 Example Use Case

```clojure
;; Initiator creates a deal with Bob for 100 STX
(initiate-deal 'SP...BOB u100)

;; Initiator completes payment
(complete-payment u1)

;; Bob rates initiator with a trust score of 5
(rate-counterparty u0 u5)
```

---

### 📘 Requirements

* Clarity smart contract environment (Stacks blockchain)
* STX for gas and payment execution

---

### 🛠️ Future Enhancements (Ideas)

* **Dispute resolution mechanism**
* **Multi-sig escrow**
* **Time-locked deals**
* **Rating feedback or comment support**

---
