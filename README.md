# Hybrid Proof-of-Work Ledger Prototype  
**Security, Economic Constraints, and Adversarial Analysis**

---

## Executive Summary
This project implements a **custom Proof-of-Work (PoW) ledger system** to study how **security, incentives, and state consistency** interact in decentralized financial infrastructures.

Rather than treating blockchain as a product, the implementation is framed as a **financial systems experiment**:  
- how to prevent **double spending and replay** without trusted intermediaries,
- how **economic constraints** mitigate resource-exhaustion attacks,
- and how **consensus rules enforce ledger integrity under adversarial behavior**.

The system combines:
- **Nakamoto-style PoW consensus** (costly history rewriting),
- an **account-based state model** (explicit balances and nonces),
- and **modern cryptography (Ed25519)** for efficient identity verification.

All security claims are validated through **explicit adversarial simulations**, not assumptions.  
This repository demonstrates hands-on reasoning about **transaction validity, risk containment, and system robustness**—skills directly relevant to financial infrastructure, payment systems, and risk engineering.

---

## Motivation: Why this system?
Conventional financial payment systems rely on **centralized intermediaries** to ensure correctness and finality. While efficient, this model concentrates operational and governance risk: failures, censorship, or misaligned incentives at the intermediary level directly affect all users.

A decentralized ledger removes the trusted intermediary—but replaces it with a harder problem:  
**how to prevent double spending and fraudulent state transitions in an adversarial environment.**

Nakamoto consensus solves this by tying ledger history to **real economic cost (PoW)**.  
This project explores what that guarantee actually requires in practice:
- What transaction rules must be enforced?
- What attacks still remain possible?
- Which constraints are economic rather than cryptographic?

The goal is not to build a new currency, but to **understand and implement the minimal rules required for financial integrity without trust.**

---

## Design Rationale (PoW + Account Model)
### Why Proof-of-Work?
PoW provides a clear and quantifiable security assumption:
> rewriting history requires proportional real-world cost.

This makes it a natural baseline for studying **attack feasibility, cost asymmetry, and incentive alignment**, which are core concerns in financial risk analysis.

### Why an Account-based State Model?
Instead of Bitcoin’s UTXO model, this system uses explicit accounts:
- `balance[address]`
- `nonce[address]`

This choice:
- simplifies reasoning about **state transitions**,
- enables strict **nonce-based replay protection**,
- and cleanly separates **consensus logic** from **transaction validity logic**.

The result is a clearer laboratory for studying **ledger consistency and fraud prevention**.

---

## Key Technical Features

### 1) Consensus and Fork Resolution
- PoW mining with adjustable difficulty
- Longest-chain (most accumulated work) rule for fork resolution

### 2) Strict Nonce Ordering (Replay & Double-Spend Protection)
- Transactions must satisfy: `Tx.nonce == Account.nonce`
- Conflicting transactions are rejected deterministically
- Ensures **idempotent state transitions**

### 3) Adaptive Precision (Deterministic State-Space Control)
- Transaction amounts must satisfy: `decimal_places(amount) ≤ current_difficulty`
- Transactions exceeding the allowed precision are rejected at validation time
- Ensures **bounded monetary granularity**, preventing uncontrolled state-space fragmentation


### 4) Modern Cryptography (Ed25519)
- Faster signing and verification
- Smaller public keys (storage efficiency)
- Strong resistance to common ECDSA implementation pitfalls

### 5) Mempool Governance (DoS Mitigation)
- Bounded pending transaction pool
- Fee-based prioritization
- Excess traffic is dropped to preserve system stability

### 6) Temporal Integrity Checks
- Reject blocks with excessive future timestamps
- Prevents difficulty manipulation and time-warp attacks

---

## Security Validation (Adversarial Simulations)

| Attack Scenario | Defense Mechanism | Result |
|---|---|---|
| Double Spending | Nonce collision + validation rules | Rejected |
| Replay Attack | Strict nonce equality | Rejected |
| DoS Flooding | Mempool size & fee priority | Mitigated |
| Time-Warp Attack | Timestamp drift checks | Rejected |

All simulations and logs are available in `Final_Project.ipynb`.

---

## Repository Contents
- **`Final_Project.ipynb`**  
  Full Python implementation:
  - Wallet (Ed25519 identities)
  - PoW consensus engine
  - State transition & validation logic
  - Adversarial simulations
- **`Cryptocurrency Report.pdf`**  
  Formal technical report covering:
  - consensus background,
  - economic security constraints,
  - and attack analysis.

---

## Limitations (Prototype Scope)
- In-memory state only (no persistent storage)
- Simulated P2P layer (no real network latency)
- Python-based mining (not performance-optimized)

This is an analytical prototype, not a production payment system.

---
