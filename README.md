# LuxFHE Solidity

**FHE-enabled Smart Contract Library for Lux Network**

[![License](https://img.shields.io/badge/license-Lux%20Research-blue.svg)](LICENSE)

---

## ⚠️ Important IP Notice

**This library uses Lux's independent FHE implementation:**

- ✅ Interfaces with `github.com/luxfi/tfhe` (our Go TFHE)
- ✅ Precompiles implemented in `lux/evm/precompile/contracts/fhe/`
- ✅ NO dependency on Zama or any third-party FHE code
- ✅ Protected by Lux Industries patent portfolio

---

## Overview

Solidity library for writing confidential smart contracts on Lux Network. Enables operations on encrypted data directly in smart contracts.

## Installation

```bash
npm install @luxfhe/solidity
# or
forge install luxfhe/solidity
```

## Quick Start

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@luxfhe/solidity/FHE.sol";

contract ConfidentialCounter {
    euint32 private counter;
    
    constructor() {
        counter = FHE.asEuint32(0);
    }
    
    function increment(inEuint32 calldata encryptedAmount) public {
        euint32 amount = FHE.asEuint32(encryptedAmount);
        counter = FHE.add(counter, amount);
    }
    
    function getEncrypted() public view returns (euint32) {
        return counter;
    }
}
```

## Encrypted Types

| Type | Description |
|------|-------------|
| `ebool` | Encrypted boolean |
| `euint8` | Encrypted 8-bit unsigned integer |
| `euint16` | Encrypted 16-bit unsigned integer |
| `euint32` | Encrypted 32-bit unsigned integer |
| `euint64` | Encrypted 64-bit unsigned integer |
| `euint128` | Encrypted 128-bit unsigned integer |
| `euint160` | Encrypted Ethereum address |
| `euint256` | Encrypted EVM word |
| `eaddress` | Encrypted address type |

## Operations

### Arithmetic
- `FHE.add(a, b)` - Addition
- `FHE.sub(a, b)` - Subtraction
- `FHE.mul(a, b)` - Multiplication

### Comparison
- `FHE.eq(a, b)` - Equal
- `FHE.ne(a, b)` - Not equal
- `FHE.lt(a, b)` - Less than
- `FHE.le(a, b)` - Less than or equal
- `FHE.gt(a, b)` - Greater than
- `FHE.ge(a, b)` - Greater than or equal

### Bitwise
- `FHE.and(a, b)` - Bitwise AND
- `FHE.or(a, b)` - Bitwise OR
- `FHE.xor(a, b)` - Bitwise XOR
- `FHE.not(a)` - Bitwise NOT
- `FHE.shl(a, n)` - Left shift
- `FHE.shr(a, n)` - Right shift

### Control Flow
- `FHE.select(condition, a, b)` - Encrypted ternary

## Precompile Addresses

| Address | Operation | Gas Cost |
|---------|-----------|----------|
| 0x0080 | FheAdd | 50,000 |
| 0x0081 | FheSub | 50,000 |
| 0x0082 | FheMul | 150,000 |
| 0x0083 | FheDiv | 200,000 |
| 0x0090 | FheEq | 30,000 |
| 0x0091 | FheLt | 30,000 |
| 0x00A0 | FheDecrypt | 500,000 |

## Contract Templates

### ConfidentialERC20

```solidity
import "@luxfhe/solidity/token/ConfidentialERC20.sol";

contract MyToken is ConfidentialERC20 {
    constructor() ConfidentialERC20("My Token", "MTK") {
        _mint(msg.sender, FHE.asEuint256(1000000 * 10**18));
    }
}
```

### ConfidentialGovernance

```solidity
import "@luxfhe/solidity/governance/ConfidentialGovernorAlpha.sol";
```

## Architecture

```
Solidity Contracts (this library)
        ↓
FHE Precompiles (lux/evm/precompile/contracts/fhe/)
        ↓
Go TFHE Library (github.com/luxfi/tfhe)
        ↓
Lattice Primitives (github.com/luxfi/lattice)
```

## License

**Lux Research License with Patent Reservation**

- ✅ Free for research and academic use
- ✅ Free on Lux Network (mainnet/testnet)
- 📧 Commercial license required for other networks

Contact: oss@lux.network

## Related

- [github.com/luxfi/tfhe](https://github.com/luxfi/tfhe) - Go TFHE implementation
- [github.com/luxfi/standard](https://github.com/luxfi/standard) - Full contract library
- [docs.lux.network/fhe](https://docs.lux.network/fhe) - Documentation

---

© 2020-2025 Lux Industries Inc. All rights reserved.
