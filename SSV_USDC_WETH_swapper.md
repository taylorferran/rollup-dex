# SSV <> USDC <> WETH Multi-Token Swapper

Swapper enables swapping between these 3 tokens on rollup B:
- **WETH**: `0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553`
- **USDC**: `0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D`
- **SSV**: `0x79155fb8d8dE01522bE1Cbd17e538966d78d1565`

tokens have 18 decimals and the swapper uses a constant product formula with a 0.3% fee, are deployed on both rollup A and B 

## Contract Details

- **Swapper Address**: `0x5c5c962e598dDCda29403Bf817470c9B839267F1`
- **Network**: Rollup B (Chain ID: 88888)
- **RPC URL**: http://57.129.73.144:31133/
- **Fee**: 0.3% (300 basis points)
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
cast call 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "getReserves()" --rpc-url http://57.129.73.144:31133/
```

### 2. Get Swap Price Preview

```bash
# Token types (for calls below): 0=WETH, 1=USDC, 2=SSV

# Preview WETH -> USDC (1 WETH)
cast call 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000 --rpc-url http://57.129.73.144:31133/

# Preview USDC -> SSV (1000 USDC)
cast call 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "getSwapPrice(uint8,uint8,uint256)" 1 2 1000000000000000000000 --rpc-url http://57.129.73.144:31133/

# Preview SSV -> WETH (10 SSV)
cast call 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "getSwapPrice(uint8,uint8,uint256)" 2 0 10000000000000000000 --rpc-url http://57.129.73.144:31133/
```

### 3. Execute Swaps

Replace `[YOUR_ADDRESS]` and use the private key below.

```bash
PK=<YOUR_PRIVATE_KEY>

# Approve tokens to the swapper once per token
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "approve(address,uint256)" 0x5c5c962e598dDCda29403Bf817470c9B839267F1 1000000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key $PK
cast send 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "approve(address,uint256)" 0x5c5c962e598dDCda29403Bf817470c9B839267F1 1000000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key $PK
cast send 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "approve(address,uint256)" 0x5c5c962e598dDCda29403Bf817470c9B839267F1 1000000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key $PK

# Swap WETH -> USDC (1 WETH)
cast send 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key $PK

# Swap USDC -> SSV (1000 USDC)
cast send 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key $PK

# Swap SSV -> WETH (10 SSV)
cast send 0x5c5c962e598dDCda29403Bf817470c9B839267F1 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key $PK
```

## Token Minting/Balances (both Rollups)

USDC and SSV are deployed with CREATE2 on both Rollup A (77777) and Rollup B (88888) at the same addresses.

### Rollup A (77777) RPC: http://57.129.73.156:31130/

```bash
PK=<YOUR_PRIVATE_KEY>

# Mint 1,000,000 USDC to yourself
cast send --rpc-url http://57.129.73.156:31130/ --private-key $PK 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000

# Mint 1,000,000 SSV to yourself
cast send --rpc-url http://57.129.73.156:31130/ --private-key $PK 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000

# Check balances
cast call --rpc-url http://57.129.73.156:31130/ 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "balanceOf(address)" [YOUR_ADDRESS]
cast call --rpc-url http://57.129.73.156:31130/ 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "balanceOf(address)" [YOUR_ADDRESS]
```

### Rollup B (88888) RPC: http://57.129.73.144:31133/

```bash
PK=<YOUR_PRIVATE_KEY>

# Mint more tokens if needed
cast send --rpc-url http://57.129.73.144:31133/ --private-key $PK 0xeA0DB94b4c702d9cA0Fcc65715A035B24dF3452D "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000
cast send --rpc-url http://57.129.73.144:31133/ --private-key $PK 0x79155fb8d8dE01522bE1Cbd17e538966d78d1565 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000

# Approvals and swaps are shown above
```

## Notes

- CREATE2 used for USDC and SSV to ensure same addresses on both rollups.
- Swapper deployed only on Rollup B.
- All tokens and swaps use 18 decimals.


