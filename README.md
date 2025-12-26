# Hybrid PoW Blockchain Protocol: Security Analysis & Implementation

## Executive Summary
This project presents a comprehensive design and security analysis of a custom **Proof-of-Work (PoW) cryptocurrency protocol**. Bridging the gap between theoretical consensus mechanisms and practical engineering, the system implements a **hybrid architecture** that combines Bitcoin’s Nakamoto Consensus with Ethereum’s Account-based State Model.

Unlike standard blockchain implementations, this research focuses on **economic security constraints** and **cryptographic modernization**. Key innovations include an "Adaptive Precision" mechanism to mitigate dust attacks and the integration of **Ed25519** signatures for enhanced performance. The protocol's resilience against **Double-Spending, Replay Attacks, and Denial of Service (DoS)** has been rigorously verified through adversarial simulations.

---

## Key Technical Innovations

### 1. Hybrid Architecture (PoW + Account Model)
I designed a hybrid ledger system to optimize state management within a probabilistic consensus framework.
* **Design Choice:** While adopting Bitcoin's PoW for decentralized consensus, I utilized an **Account-based Model** (mapping addresses to balances/nonces) instead of UTXO.
* **Advantage:** This decouples consensus logic from state transitions, allowing for $O(1)$ balance lookups and simplifying the implementation of complex transaction logic compared to stateless UTXO models.

### 2. Adaptive Precision Mechanism (Economic Security)
To prevent ledger bloat and "dust spam" (micro-transactions that clog the network), I implemented a novel economic constraint linked to mining difficulty.
* **Mechanism:** The allowed number of decimal places for a transaction is dynamically capped by the current network difficulty ($D$).
  $$\text{Allowed Decimals} \le D$$
* **Impact:** As the network grows (difficulty increases), the currency naturally becomes more divisible. This organic scaling prevents spam attacks during the network's early, vulnerable stages.

### 3. Modern Cryptography (Ed25519)
Moving beyond the legacy `secp256k1` curve used by Bitcoin, this protocol employs the **Ed25519** signature scheme.
* **Performance:** Offers faster signing/verification operations and smaller public keys (32 bytes), optimizing block storage and throughput.
* **Security:** Provides inherent resistance against side-channel attacks that affect standard ECDSA implementations.

---

## Security Audit & Simulation Results

The system was subjected to a series of adversarial simulations to verify its robustness. (See `Final_Project.ipynb` for full logs).

| Attack Vector | Countermeasure Implementation | Simulation Outcome |
| :--- | :--- | :--- |
| **Double Spending** | **Mempool Collision & Longest Chain Rule** <br> The system detects conflicting nonces in the mempool and rejects the second transaction. |  **Blocked** (Tx Rejected) |
| **Replay Attack** | **Strict Nonce Ordering** <br> Enforced `Tx.Nonce == Account.Nonce`. Attempting to rebroadcast a signed Tx fails as the account state increments. |  **Blocked** (Nonce Mismatch) |
| **DoS Flooding** | **Mempool Governance** <br> Implemented `max_pending_size` and fee-based prioritization. Excess transactions are dropped to preserve RAM. |  **Mitigated** (Traffic Dropped) |
| **Time-Warp** | **Temporal Drift Checks** <br> Blocks with timestamps > 2 hours into the future are rejected to maintain difficulty integrity. |  **Validated** |

---

## Repository Structure

* **`Cryptocurrency Report.pdf`**:
    **[Official Report]** A detailed 17-page technical paper covering the theoretical background, architectural decisions, and comprehensive security audit of the protocol.
* **`Final_Project.ipynb`**:
    The core implementation notebook containing the full source code:
    * **`Wallet`**: Identity management via Ed25519.
    * **`PoWChain`**: Consensus engine and difficulty adjustment.
    * **`TransactionValidator`**: Logic for state updates and adaptive precision.
    * **`Node`**: P2P simulation and fork resolution logic.

---

## Future Work & Limitations
* **Persistence:** Currently, the ledger state is maintained in volatile memory (RAM). Future iterations will integrate **LevelDB** for on-disk persistence.
* **Network Stack:** The P2P layer is currently simulated via object interaction. A full **TCP/IP stack** is required for real-world latency modeling.
* **Mining Efficiency:** The mining loop is implemented in Python. For production, this module should be optimized in **C++** or designed to be ASIC-resistant.

## Disclaimer

This is a prototype implementation designed for educational purposes. It is NOT suitable for production use or handling real financial transactions. The code has known limitations and has not undergone professional security auditing.
