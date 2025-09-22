# MTK1, MTK2, WETH Multi-Token Swapper

## Overview

This multi-token swapper enables seamless swapping between three tokens on rollup B:
- **MTK1**: `0x68278160572a0D0bcca3A3faC6DBC93Aff7e950F`
- **MTK2**: `0x0D4F9d2dDB8C1F571D8C26B67B55d62Efc502893`
- **WETH**: `0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553`

All tokens have 18 decimals and the swapper uses a constant product formula with a 0.3% fee.

## Contract Details

- **Swapper Address**: `0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70`
- **Network**: Rollup B (Chain ID: 88888)
- **RPC URL**: http://57.129.73.144:31133/
- **Fee**: 0.3% (300 basis points)
- **Solidity Version**: 0.7.6

## Available Swap Pairs

The swapper supports all possible combinations between the three tokens:

1. **MTK1 ↔ MTK2**
2. **MTK1 ↔ WETH**
3. **MTK2 ↔ WETH**

## Contract Functions

### Core Functions

#### `getSwapPrice(TokenType tokenIn, TokenType tokenOut, uint256 amountIn)`
Returns the expected output amount and price for a swap.
- **Parameters**:
  - `tokenIn`: Input token type (MTK1, MTK2, or WETH)
  - `tokenOut`: Output token type (MTK1, MTK2, or WETH)
  - `amountIn`: Amount of input tokens
- **Returns**: `(uint256 amountOut, uint256 price)`

#### `swap(address recipient, TokenType tokenIn, TokenType tokenOut, uint256 amountIn)`
Generic swap function for any token pair.
- **Parameters**:
  - `recipient`: Address to receive output tokens
  - `tokenIn`: Input token type
  - `tokenOut`: Output token type
  - `amountIn`: Amount of input tokens

#### `getReserves()`
Returns current token balances in the swapper.
- **Returns**: `(uint256 mtk1Reserve, uint256 mtk2Reserve, uint256 wethReserve)`

### Convenience Functions

#### MTK1 ↔ MTK2
- `swapMTK1ForMTK2(address recipient, uint256 amountIn)`
- `swapMTK2ForMTK1(address recipient, uint256 amountIn)`

#### MTK1 ↔ WETH
- `swapMTK1ForWETH(address recipient, uint256 amountIn)`
- `swapWETHForMTK1(address recipient, uint256 amountIn)`

#### MTK2 ↔ WETH
- `swapMTK2ForWETH(address recipient, uint256 amountIn)`
- `swapWETHForMTK2(address recipient, uint256 amountIn)`

### Administrative Functions

#### `addLiquidity(uint256 mtk1Amount, uint256 mtk2Amount, uint256 wethAmount)`
Adds liquidity to the swapper (requires approval first).

#### `withdrawTokens(address token, uint256 amount)`
Emergency function to withdraw tokens from the swapper.

## Usage Examples

### 1. Check Current Reserves

```bash
# Using cast
cast call 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 "getReserves()" --rpc-url http://57.129.73.144:31133/
```

### 2. Get Swap Price Preview

```bash
# Preview MTK1 -> MTK2 swap (1000 tokens)
cast call 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000000 --rpc-url http://57.129.73.144:31133/

# Token types: 0=MTK1, 1=MTK2, 2=WETH
```

### 3. Execute Swaps

#### MTK1 -> MTK2
```bash
# First approve the swapper to spend your MTK1
cast send 0x68278160572a0D0bcca3A3faC6DBC93Aff7e950F "approve(address,uint256)" 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key PRIVAT_KEY

# Then execute the swap
cast send 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 "swapMTK1ForMTK2(address,uint256)" 0x4da9f34f83d608cAB03868662e93c96Bc9793495 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key PRIVAT_KEY
```

#### MTK2 -> WETH
```bash
# Approve MTK2
cast send 0x0D4F9d2dDB8C1F571D8C26B67B55d62Efc502893 "approve(address,uint256)" 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key PRIVAT_KEY

# Execute swap
cast send 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 "swapMTK2ForWETH(address,uint256)" 0x4da9f34f83d608cAB03868662e93c96Bc9793495 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key PRIVAT_KEY
```

#### WETH -> MTK1
```bash
# Approve WETH
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "approve(address,uint256)" 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key PRIVAT_KEY

# Execute swap
cast send 0x18d2d78f8e01b0777075a7A2D4A1C677641A9E70 "swapWETHForMTK1(address,uint256)" 0x4da9f34f83d608cAB03868662e93c96Bc9793495 1000000000000000000000 --rpc-url http://57.129.73.144:31133/ --private-key PRIVAT_KEY
```

## Price Calculation

The swapper uses a constant product formula with fees:

```
amountInWithFee = amountIn * (10000 - 300) / 10000  // 0.3% fee
amountOut = (amountInWithFee * reserveOut) / (reserveIn + amountInWithFee)
```

## Token Information

### MTK1
- **Address**: `0x68278160572a0D0bcca3A3faC6DBC93Aff7e950F`
- **Name**: MyToken1
- **Symbol**: MTK1
- **Decimals**: 18
- **Features**: Mintable, Burnable

### MTK2
- **Address**: `0x0D4F9d2dDB8C1F571D8C26B67B55d62Efc502893`
- **Name**: MyToken2
- **Symbol**: MTK2
- **Decimals**: 18
- **Features**: Mintable, Burnable

### WETH
- **Address**: `0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553`
- **Name**: Wrapped Ether
- **Symbol**: WETH
- **Decimals**: 18
- **Features**: Wrap/Unwrap ETH, Mintable, Burnable
