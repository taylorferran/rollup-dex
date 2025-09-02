# Complete Trading Commands

## 🚀 **MultiTokenSwapper Trading Sequence**

All commands use the private key: `[YOUR_PRIVATE_KEY]`

### **Step 1: Mint 1,000,000 USDC**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x1d62c73518791a7276b432d61ed11b41564b578a "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000
```

### **Step 2: Check USDC Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x1d62c73518791a7276b432d61ed11b41564b578a "balanceOf(address)" [YOUR_ADDRESS]
```

### **Step 3: Approve USDC to the Swapper Contract**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x1d62c73518791a7276b432d61ed11b41564b578a "approve(address,uint256)" 0x52cfc57b976936ba8d6beea547900c4425836bea 1000000000000000000000000
```

### **Step 4: Swap 1,000 USDC to SSV**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000
```

### **Step 5: Check SSV Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0xf4b3f33aa84b8ae0cb1de041cc84a7713785e327 "balanceOf(address)" [YOUR_ADDRESS]
```

### **Step 6: Approve SSV to the Swapper Contract**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0xf4b3f33aa84b8ae0cb1de041cc84a7713785e327 "approve(address,uint256)" 0x52cfc57b976936ba8d6beea547900c4425836bea 1000000000000000000000000
```

### **Step 7: Swap 10 SSV to WETH**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000
```

### **Step 8: Check WETH Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0xb64f139094bffdf506986e528b013f55689990c5 "balanceOf(address)" [YOUR_ADDRESS]
```
## 📋 **Command Summary**

| Step | Operation | Amount | Token | Expected Result |
|------|-----------|--------|-------|-----------------|
| 1 | Mint | 1,000,000 | USDC | Receive 1,000,000 USDC |
| 2 | Check | - | USDC | Verify balance |
| 3 | Approve | ∞ | USDC | USDC approved for unlimited swaps |
| 4 | Swap | 1,000 | USDC→SSV | Receive ~8,860 SSV |
| 5 | Check | - | SSV | Verify SSV balance |
| 6 | Approve | ∞ | SSV | SSV approved for unlimited swaps |
| 7 | Swap | 10 | SSV→WETH | Receive ~43.8 WETH |
| 8 | Check | - | WETH | Verify WETH balance |

## 🔧 **Contract Addresses**

- **WETH**: `0xb64f139094bffdf506986e528b013f55689990c5`
- **USDC**: `0x1d62c73518791a7276b432d61ed11b41564b578a`
- **SSV**: `0xf4b3f33aa84b8ae0cb1de041cc84a7713785e327`
- **MultiTokenSwapper**: `0x52cfc57b976936ba8d6beea547900c4425836bea`




## **Swap Function:**
```solidity
swap(address recipient, uint8 tokenIn, uint8 tokenOut, uint256 amountIn)
```

### **Parameters:**
- `recipient` - Address that receives the output tokens
- `tokenIn` - Token you're swapping FROM (0, 1, or 2)
- `tokenOut` - Token you're swapping TO (0, 1, or 2)
- `amountIn` - Amount to swap (in wei, 18 decimals)

### **Example Swaps:**
```bash
# USDC → SSV (1000 USDC)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000

# SSV → WETH (10 SSV)  
cast send --rpc-url http://57.129.73.144:31133/ --private-key [KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000

# WETH → USDC (1 WETH)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000
```

## **Price Preview Function:**
```solidity
getSwapPrice(uint8 tokenIn, uint8 tokenOut, uint256 amountIn) → (uint256 amountOut, uint256 price)
```

### **Returns:**
- `amountOut` - How many tokens you'll receive
- `price` - Price ratio (with 18 decimal precision)

### **Example Price Checks:**
```bash
# Check: How much SSV for 1000 USDC?
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getSwapPrice(uint8,uint8,uint256)" 1 2 1000000000000000000000

# Check: How much WETH for 10 SSV?
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getSwapPrice(uint8,uint8,uint256)" 2 0 10000000000000000000

# Check: How much USDC for 1 WETH?
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000
```

## **Quick Reference:**
| Operation | Format | Example |
|-----------|---------|---------|
| **USDC → SSV** | `1 2 amount` | `1 2 1000000000000000000000` |
| **SSV → WETH** | `2 0 amount` | `2 0 10000000000000000000` |
| **WETH → USDC** | `0 1 amount` | `0 1 1000000000000000000` |

**⚠️ Remember:** Always approve tokens first and check prices before swapping!

## 🌐 **Network Details**

- **Network**: Rollup B
- **Chain ID**: 88888
- **RPC URL**: http://57.129.73.144:31133/
- **Private Key**: [YOUR_PRIVATE_KEY]

## 🔍 **Swap Function Parameter Explanation**

### **Generic Swap Function:**
```bash
cast send --rpc-url [RPC_URL] --private-key [PRIVATE_KEY] [SWAPPER_ADDRESS] "swap(address,uint8,uint8,uint256)" [RECIPIENT] [TOKEN_IN] [TOKEN_OUT] [AMOUNT_IN]
```

### **Parameter Breakdown:**

| Parameter | Type | Description | Example Values |
|-----------|------|-------------|----------------|
| **RECIPIENT** | `address` | Address that receives the output tokens | `[YOUR_ADDRESS]` |
| **TOKEN_IN** | `uint8` | Token type you're swapping FROM (0=WETH, 1=USDC, 2=SSV) | `1` (USDC) |
| **TOKEN_OUT** | `uint8` | Token type you're swapping TO (0=WETH, 1=USDC, 2=SSV) | `2` (SSV) |
| **AMOUNT_IN** | `uint256` | Amount of input tokens (in wei) | `1000000000000000000000` (1000 tokens) |

### **Token Type Mapping:**

| Token | Enum Value | Address |
|-------|------------|---------|
| **WETH** | 0 | `0xb64f139094bffdf506986e528b013f55689990c5` |
| **USDC** | 1 | `0x1d62c73518791a7276b432d61ed11b41564b578a` |
| **SSV** | 2 | `0xf4b3f33aa84b8ae0cb1de041cc84a7713785e327` |

### **Example Swap Commands:**

```bash
# USDC → SSV (1=USDC, 2=SSV)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000

# SSV → WETH (2=SSV, 0=WETH)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000

# WETH → USDC (0=WETH, 1=USDC)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x52cfc57b976936ba8d6beea547900c4425836bea "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000000
```

## 🎯 **Expected Flow for this example**

1. **Mint USDC** → Get 1,000,000 USDC
2. **Check USDC** → Verify you have enough
3. **Approve USDC** → Allow swapper to spend USDC
4. **Swap USDC→SSV** → Get ~8,860 SSV
5. **Check SSV** → Verify SSV received
6. **Approve SSV** → Allow swapper to spend SSV
7. **Swap SSV→WETH** → Get ~43.8 WETH
8. **Check WETH** → Verify WETH received

## 💡 **Additional Commands**

### **Price Preview (Check before swapping):**
```bash
# Preview USDC → SSV swap
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getSwapPrice(uint8,uint8,uint256)" 1 2 1000000000000000000000

# Preview SSV → WETH swap
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getSwapPrice(uint8,uint8,uint256)" 2 0 10000000000000000000

# Preview WETH → USDC swap
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000000
```

### **Check Swapper Reserves:**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x52cfc57b976936ba8d6beea547900c4425836bea "getReserves()"
```

### **Mint Additional Tokens:**
```bash
# Mint 1000 WETH
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0xb64f139094bffdf506986e528b013f55689990c5 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000

# Mint 1000 SSV
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0xf4b3f33aa84b8ae0cb1de041cc84a7713785e327 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000
```

### **Burn Tokens (if needed):**
```bash
# Burn 100 USDC
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x1d62c73518791a7276b432d61ed11b41564b578a "burn(address,uint256)" [YOUR_ADDRESS] 100000000000000000000

# Burn 100 SSV
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0xf4b3f33aa84b8ae0cb1de041cc84a7713785e327 "burn(address,uint256)" [YOUR_ADDRESS] 100000000000000000000
```



## ⚠️ **Important Notes**

- **All tokens use 18 decimals** (1 token = 1000000000000000000 wei)
- **Fee**: 0.3% fee applied to all swaps
- **Slippage**: No built-in slippage protection - use `getSwapPrice()` to check expected output
- **Approvals**: Only need to approve once per token for unlimited swaps
- **Chain ID**: 88888 (New Rollup B)
