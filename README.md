# Cryptocurrency Protocol Implementation

A comprehensive Python-based implementation of a Proof-of-Work (PoW) blockchain system with enhanced security features and adaptive precision mechanisms.

## Overview

This project implements a hybrid cryptocurrency protocol that combines Bitcoin's Proof-of-Work consensus mechanism with an Ethereum-style account-based state model. The implementation serves as an educational tool for understanding distributed consensus systems and blockchain security.

## Key Features

### Core Functionality
- **Proof-of-Work Consensus**: SHA-256 based mining with dynamic difficulty adjustment
- **Account-based Model**: Simplified state management with direct address-to-balance mapping
- **Ed25519 Signatures**: Modern elliptic curve cryptography for efficient key management
- **Merkle Tree Integration**: Transaction integrity verification with lightweight client support
- **Fee Market Mechanism**: Transaction prioritization based on fee density

### Security Features
- **Replay Attack Prevention**: Strictly increasing nonce system for transaction ordering
- **DoS Mitigation**: Mempool size limits and transaction expiration mechanisms
- **Temporal Integrity**: Timestamp validation to prevent time-warp attacks
- **Fork Resolution**: Longest chain rule implementation with automatic reorganization

### Innovative Mechanisms
- **Adaptive Precision**: Dynamic adjustment of minimum transaction units based on network difficulty
- **Dynamic Difficulty**: Automatic adjustment every 5 blocks to maintain target block time
- **Chain Reorganization**: Full state replay verification during fork resolution

## Technical Architecture

### Core Components

```
Wallet
├── Ed25519 key pair generation
├── Base58Check address encoding
└── Transaction signing

Transaction System
├── CoinbaseTx (mining rewards)
└── AccountTx (peer-to-peer transfers)

Block Structure
├── Block header (prev_hash, merkle_root, timestamp, nonce, difficulty)
└── Transaction list

State Management
├── StateManager (balances and nonces)
├── MempoolManager (pending transactions)
└── TransactionValidator (consensus rules)

Consensus Engine
├── PoWChain (main controller)
├── Mining algorithm
└── Fork resolution logic
```

## Installation

### Prerequisites
- Python 3.8+
- Required packages:
  - `cryptography`
  - `base58`
  - `hashlib` (standard library)
  - `dataclasses` (standard library)

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/cryptocurrency-protocol.git
cd cryptocurrency-protocol

# Install dependencies
pip install cryptography base58

# Run the implementation
jupyter notebook Final_Project.ipynb
```

## Usage Examples

### Creating Wallets and Transactions

```python
# Create wallets
alice = Wallet()
bob = Wallet()
miner = Wallet()

# Create a transaction
tx = AccountTx(
    sender=alice.address,
    recipient=bob.address,
    amount=Decimal("10.5"),
    fee=Decimal("0.1"),
    nonce=0
)
tx.sign(alice)

# Add to blockchain
blockchain.add_transaction(tx)
```

### Mining Blocks

```python
# Initialize blockchain
blockchain = PoWChain()

# Mine a block
block = blockchain.mine_block(miner)
print(f"Block mined with difficulty: {block.difficulty}")
print(f"Block hash: {block.blockhash()}")
```

### Handling Chain Reorganization

```python
# Simulate fork resolution
node_a = PoWChain()
node_b = PoWChain()

# Node A mines 3 blocks, Node B mines 1 block
# ...

# Node B synchronizes with Node A
success = node_b.attempt_reorg(node_a.chain)
print(f"Reorganization successful: {success}")
```

## Security Analysis

### Implemented Protections

**DoS Attack Mitigation**
- Maximum mempool size limit (default: 1000 transactions)
- Per-sender transaction limits
- Minimum fee requirements
- Automatic transaction expiration after 100 blocks

**Replay Attack Prevention**
- Strictly ordered nonce system
- Signature verification on all transactions
- State-dependent transaction validation

**Time-Warp Attack Prevention**
- Maximum future drift: 2 hours
- Chronological timestamp ordering
- Difficulty adjustment bounds

## Performance Characteristics

### Mining Difficulty Adjustment
- Target block time: 1.0 second
- Adjustment interval: 5 blocks
- Algorithm: Negative feedback loop based on actual vs. expected time

### Transaction Validation
- Ed25519 signature verification: ~0.001s per transaction
- Merkle root calculation: O(n log n) where n = number of transactions
- State lookup: O(1) with dictionary-based storage

## Known Limitations

### Production Readiness Gaps

1. **Data Persistence**: Currently uses in-memory storage (volatile)
   - Recommendation: Integrate LevelDB or RocksDB

2. **Network Layer**: No real P2P implementation
   - Recommendation: Implement libp2p or custom asyncio-based networking

3. **Mining Efficiency**: Pure Python implementation (GIL-limited)
   - Recommendation: Rewrite PoW loop in C++ or Rust

## Testing

The implementation includes comprehensive simulation scenarios:
- Standard transaction flow and mining
- Double-spending prevention
- Fork resolution and chain reorganization
- Security attack simulations (DoS, Replay, Time-warp)

Run all tests via the Jupyter notebook or extract test cases to a separate test suite.

## Academic Context

This implementation was developed as part of a research project on cryptocurrency systems and distributed consensus mechanisms. For detailed technical analysis, please refer to the accompanying research paper: `Cryptocurrency_Report.pdf`.

## References

1. Nakamoto, S. (2008). Bitcoin: A Peer-to-Peer Electronic Cash System
2. Buterin, V. (2014). Ethereum: A Next-Generation Smart Contract and Decentralized Application Platform
3. Johnson, D., Menezes, A., & Vanstone, S. (2001). The Elliptic Curve Digital Signature Algorithm (ECDSA)

## License

This project is released for educational and research purposes. Please cite appropriately if used in academic work.

## Contributing

This is an academic project, but suggestions and improvements are welcome. Please open an issue or submit a pull request.

## Disclaimer

This is a prototype implementation designed for educational purposes. It is NOT suitable for production use or handling real financial transactions. The code has known limitations and has not undergone professional security auditing.
