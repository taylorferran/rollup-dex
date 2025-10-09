# SSV, USDC, WETH Multi-Token Swapper - New Chains Deployment

## Overview

Swapper enables swapping between these 3 tokens on rollup B:
- **WETH**: `0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396`
- **USDC**: `0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D`
- **SSV**: `0x79155fb8d8dE01522bE1Cbd17e538966d78d1565`

Tokens have 18 decimals and the swapper uses a constant product formula with a 0.3% fee, are deployed on both rollup A and B.

## Contract Details

- **Swapper Address**: `0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E`
- **Network**: Rollup B (Chain ID: 222222)
- **RPC URL**: https://rpc-b.testnet.compose.network
- **Fee**: 0.3%
- **Solidity Version**: 0.7.6

## Available Swap Pairs

- WETH ↔ SSV
- SSV ↔ USDC
- USDC ↔ WETH

## Contract Functions

### Core Functions

#### `getSwapPrice(uint8 tokenIn, uint8 tokenOut, uint256 amountIn)`
Returns the expected output amount and price for a swap.

#### `swap(address recipient, uint8 tokenIn, uint8 tokenOut, uint256 amountIn)`
Generic swap function for any token pair.

#### `getReserves()`
Returns current token balances in the swapper as `(wethReserve, usdcReserve, ssvReserve)`.

## Usage Examples

### 1. Check Current Reserves

```bash
cast call 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "getReserves()" --rpc-url https://rpc-b.testnet.compose.network --insecure
```

### 2. Get Swap Price Preview

```bash
# Token types (for calls below): 0=WETH, 1=USDC, 2=SSV

# Preview WETH -> USDC (1 WETH)
cast call 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure

# Preview USDC -> SSV (1000 USDC)
cast call 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "getSwapPrice(uint8,uint8,uint256)" 1 2 1000000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure

# Preview SSV -> WETH (10 SSV)
cast call 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "getSwapPrice(uint8,uint8,uint256)" 2 0 10000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure
```

### 3. Execute Swaps

Replace `[YOUR_ADDRESS]` and use the private key below.

```bash
PK=<YOUR_PRIVATE_KEY>

# Approve tokens to the swapper once per token
cast send 0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396 "approve(address,uint256)" 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E 1000000000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK
cast send 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "approve(address,uint256)" 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E 1000000000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK
cast send 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "approve(address,uint256)" 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E 1000000000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK

# Swap WETH -> USDC (1 WETH)
cast send 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK

# Swap USDC -> SSV (1000 USDC)
cast send 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK

# Swap SSV -> WETH (10 SSV)
cast send 0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000 --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK
```

## Token Minting/Balances (both Rollups)

WETH, USDC and SSV are deployed with CREATE2 on both Rollup A (111111) and Rollup B (222222) at the same addresses.

### Rollup A (111111) RPC: https://rpc-a.testnet.compose.network

```bash
PK=<YOUR_PRIVATE_KEY>

# Mint 1,000,000 WETH to yourself
cast send --rpc-url https://rpc-a.testnet.compose.network --insecure --private-key $PK 0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000

# Mint 1,000,000 USDC to yourself
cast send --rpc-url https://rpc-a.testnet.compose.network --insecure --private-key $PK 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000

# Mint 1,000,000 SSV to yourself
cast send --rpc-url https://rpc-a.testnet.compose.network --insecure --private-key $PK 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000

# Check balances
cast call --rpc-url https://rpc-a.testnet.compose.network --insecure 0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396 "balanceOf(address)" [YOUR_ADDRESS]
cast call --rpc-url https://rpc-a.testnet.compose.network --insecure 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "balanceOf(address)" [YOUR_ADDRESS]
cast call --rpc-url https://rpc-a.testnet.compose.network --insecure 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "balanceOf(address)" [YOUR_ADDRESS]
```

### Rollup B (222222) RPC: https://rpc-b.testnet.compose.network

```bash
PK=<YOUR_PRIVATE_KEY>

# Mint more tokens if needed
cast send --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK 0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000
cast send --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000
cast send --rpc-url https://rpc-b.testnet.compose.network --insecure --private-key $PK 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000

# Approvals and swaps are shown above
```

## Deployment Summary

### Successfully Deployed Contracts:

**Rollup A (111111):**
- WETH: `0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396`
- USDC: `0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D`
- SSV: `0x79155fb8d8dE01522bE1Cbd17e538966d78d1565`

**Rollup B (222222):**
- WETH: `0xe5479bcaCf8980c9d9a7653B12288A4e6d3D5396`
- USDC: `0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D`
- SSV: `0x79155fb8d8dE01522bE1Cbd17e538966d78d1565`
- **Swapper**: `0xe24f8B8f0BCE54a9cB67b3Ec070c3c530a06647E`

