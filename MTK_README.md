# Complete Trading Commands

## 🚀 **MTK MultiTokenSwapper Trading Sequence**

All commands use the private key: `[YOUR_PRIVATE_KEY]`

### **Step 1: Mint 1,000,000 MTK1**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x68278160572a0d0bcca3a3fac6dbc93aff7e950f "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000
```

### **Step 2: Check MTK1 Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x68278160572a0d0bcca3a3fac6dbc93aff7e950f "balanceOf(address)" [YOUR_ADDRESS]
```

### **Step 3: Approve MTK1 to the Swapper Contract**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x68278160572a0d0bcca3a3fac6dbc93aff7e950f "approve(address,uint256)" 0x48788c254a7fc36f4f089d798d522089184b9650 1000000000000000000000000
```

### **Step 4: Swap 1,000 MTK1 to MTK2**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x48788c254a7fc36f4f089d798d522089184b9650 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000000
```

### **Step 5: Check MTK2 Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x0d4f9d2ddb8c1f571d8c26b67b55d62efc502893 "balanceOf(address)" [YOUR_ADDRESS]
```

### **Step 6: Approve MTK2 to the Swapper Contract**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x0d4f9d2ddb8c1f571d8c26b67b55d62efc502893 "approve(address,uint256)" 0x48788c254a7fc36f4f089d798d522089184b9650 1000000000000000000000000
```

### **Step 7: Swap 1,000 MTK2 to MTK1**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x48788c254a7fc36f4f089d798d522089184b9650 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 0 1000000000000000000000
```

### **Step 8: Check MTK1 Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x68278160572a0d0bcca3a3fac6dbc93aff7e950f "balanceOf(address)" [YOUR_ADDRESS]
```

## 📋 **Command Summary**

| Step | Operation | Amount | Token | Expected Result |
|------|-----------|--------|-------|-----------------|
| 1 | Mint | 1,000,000 | MTK1 | Receive 1,000,000 MTK1 |
| 2 | Check | - | MTK1 | Verify balance |
| 3 | Approve | ∞ | MTK1 | MTK1 approved for unlimited swaps |
| 4 | Swap | 1,000 | MTK1→MTK2 | Receive ~997.5 MTK2 |
| 5 | Check | - | MTK2 | Verify MTK2 balance |
| 6 | Approve | ∞ | MTK2 | MTK2 approved for unlimited swaps |
| 7 | Swap | 1,000 | MTK2→MTK1 | Receive ~997.5 MTK1 |
| 8 | Check | - | MTK1 | Verify MTK1 balance |

## 🔧 **Contract Addresses**

- **MTK1**: `0x68278160572a0d0bcca3a3fac6dbc93aff7e950f`
- **MTK2**: `0x0d4f9d2ddb8c1f571d8c26b67b55d62efc502893`
- **MTKMultiTokenSwapper**: `0x48788c254a7fc36f4f089d798d522089184b9650`

## **Swap Function:**
```solidity
swap(address recipient, uint8 tokenIn, uint8 tokenOut, uint256 amountIn)
```

### **Parameters:**
- `recipient` - Address that receives the output tokens
- `tokenIn` - Token you're swapping FROM (0=MTK1, 1=MTK2)
- `tokenOut` - Token you're swapping TO (0=MTK1, 1=MTK2)
- `amountIn` - Amount to swap (in wei, 18 decimals)

### **Example Swaps:**
```bash
# MTK1 → MTK2 (1000 MTK1)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [KEY] 0x48788c254a7fc36f4f089d798d522089184b9650 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000000

# MTK2 → MTK1 (1000 MTK2)  
cast send --rpc-url http://57.129.73.144:31133/ --private-key [KEY] 0x48788c254a7fc36f4f089d798d522089184b9650 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 0 1000000000000000000000
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
# Check: How much MTK2 for 1000 MTK1?
cast call --rpc-url http://57.129.73.144:31133/ 0x48788c254a7fc36f4f089d798d522089184b9650 "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000000

# Check: How much MTK1 for 1000 MTK2?
cast call --rpc-url http://57.129.73.144:31133/ 0x48788c254a7fc36f4f089d798d522089184b9650 "getSwapPrice(uint8,uint8,uint256)" 1 0 1000000000000000000000
```

## **Quick Reference:**
| Operation | Format | Example |
|-----------|---------|---------|
| **MTK1 → MTK2** | `0 1 amount` | `0 1 1000000000000000000000` |
| **MTK2 → MTK1** | `1 0 amount` | `1 0 1000000000000000000000` |

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
| **TOKEN_IN** | `uint8` | Token type you're swapping FROM (0=MTK1, 1=MTK2) | `0` (MTK1) |
| **TOKEN_OUT** | `uint8` | Token type you're swapping TO (0=MTK1, 1=MTK2) | `1` (MTK2) |
| **AMOUNT_IN** | `uint256` | Amount of input tokens (in wei) | `1000000000000000000000` (1000 tokens) |

### **Token Type Mapping:**

| Token | Enum Value | Address |
|-------|------------|---------|
| **MTK1** | 0 | `0x68278160572a0d0bcca3a3fac6dbc93aff7e950f` |
| **MTK2** | 1 | `0x0d4f9d2ddb8c1f571d8c26b67b55d62efc502893` |

### **Example Swap Commands:**

```bash
# MTK1 → MTK2 (0=MTK1, 1=MTK2)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x48788c254a7fc36f4f089d798d522089184b9650 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000000

# MTK2 → MTK1 (1=MTK2, 0=MTK1)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x48788c254a7fc36f4f089d798d522089184b9650 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 0 1000000000000000000000
```

## 🎯 **Expected Flow for this example**

1. **Mint MTK1** → Get 1,000,000 MTK1
2. **Check MTK1** → Verify you have enough
3. **Approve MTK1** → Allow swapper to spend MTK1
4. **Swap MTK1→MTK2** → Get ~997.5 MTK2
5. **Check MTK2** → Verify MTK2 received
6. **Approve MTK2** → Allow swapper to spend MTK2
7. **Swap MTK2→MTK1** → Get ~997.5 MTK1
8. **Check MTK1** → Verify MTK1 received

## 💡 **Additional Commands**

### **Price Preview (Check before swapping):**
```bash
# Preview MTK1 → MTK2 swap
cast call --rpc-url http://57.129.73.144:31133/ 0x48788c254a7fc36f4f089d798d522089184b9650 "getSwapPrice(uint8,uint8,uint256)" 0 1 1000000000000000000000

# Preview MTK2 → MTK1 swap
cast call --rpc-url http://57.129.73.144:31133/ 0x48788c254a7fc36f4f089d798d522089184b9650 "getSwapPrice(uint8,uint8,uint256)" 1 0 1000000000000000000000
```

### **Check Swapper Reserves:**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x48788c254a7fc36f4f089d798d522089184b9650 "getReserves()"
```

### **Mint Additional Tokens:**
```bash
# Mint 1000 MTK1
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x68278160572a0d0bcca3a3fac6dbc93aff7e950f "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000

# Mint 1000 MTK2
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x0d4f9d2ddb8c1f571d8c26b67b55d62efc502893 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000
```

### **Burn Tokens (if needed):**
```bash
# Burn 100 MTK1
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x68278160572a0d0bcca3a3fac6dbc93aff7e950f "burn(address,uint256)" [YOUR_ADDRESS] 100000000000000000000

# Burn 100 MTK2
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x0d4f9d2ddb8c1f571d8c26b67b55d62efc502893 "burn(address,uint256)" [YOUR_ADDRESS] 100000000000000000000
```

## ⚠️ **Important Notes**

- **All tokens use 18 decimals** (1 token = 1000000000000000000 wei)
- **Fee**: 0.3% fee applied to all swaps
- **Slippage**: No built-in slippage protection - use `getSwapPrice()` to check expected output
- **Approvals**: Only need to approve once per token for unlimited swaps
- **Chain ID**: 88888 (Rollup B)
- **CREATE2**: Both tokens deployed with CREATE2 for deterministic addresses across chains
