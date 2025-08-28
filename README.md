# Complete Trading Commands

## 🚀 **MultiTokenSwapper Trading Sequence**

All commands use the private key: `[YOUR_PRIVATE_KEY]`

### **Step 1: Mint 1,000,000 USDC**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0xB64F139094bFFdF506986E528b013F55689990c5 "mint(address,uint256)" [YOUR_ADDRESS] 1000000000000000000000000
```

### **Step 2: Check USDC Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0xB64F139094bFFdF506986E528b013F55689990c5 "balanceOf(address)" [YOUR_ADDRESS]
```

### **Step 3: Approve USDC to the Swapper Contract**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0xB64F139094bFFdF506986E528b013F55689990c5 "approve(address,uint256)" 0x2010357CafE91f56fB0373B02B44b630E3d57836 1000000000000000000000000
```

### **Step 4: Swap 1,000 USDC to SSV**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x2010357CafE91f56fB0373B02B44b630E3d57836 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000
```

### **Step 5: Check SSV Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0x1d62C73518791a7276B432d61Ed11B41564B578a "balanceOf(address)" [YOUR_ADDRESS]
```

### **Step 6: Approve SSV to the Swapper Contract**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x1d62C73518791a7276B432d61Ed11B41564B578a "approve(address,uint256)" 0x2010357CafE91f56fB0373B02B44b630E3d57836 1000000000000000000000000
```

### **Step 7: Swap 10 SSV to WETH**
```bash
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x2010357CafE91f56fB0373B02B44b630E3d57836 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000
```

### **Step 8: Check WETH Balance (Optional)**
```bash
cast call --rpc-url http://57.129.73.144:31133/ 0xA4B88d592d631dd7316140595d6022Ff024742ed "balanceOf(address)" [YOUR_ADDRESS]
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
| 7 | Swap | 10 | SSV→WETH | Receive ~437.8 WETH |
| 8 | Check | - | WETH | Verify WETH balance |

## 🔧 **Contract Addresses**

- **USDC**: `0xB64F139094bFFdF506986E528b013F55689990c5`
- **SSV**: `0x1d62C73518791a7276B432d61Ed11B41564B578a`
- **WETH**: `0xA4B88d592d631dd7316140595d6022Ff024742ed`
- **MultiTokenSwapper**: `0x2010357CafE91f56fB0373B02B44b630E3d57836`

## 🌐 **Network Details**

- **Network**: Rollup B
- **Chain ID**: 66666
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
| **WETH** | 0 | `0xA4B88d592d631dd7316140595d6022Ff024742ed` |
| **USDC** | 1 | `0xB64F139094bFFdF506986E528b013F55689990c5` |
| **SSV** | 2 | `0x1d62C73518791a7276B432d61Ed11B41564B578a` |

### **Example Swap Commands:**

```bash
# USDC → SSV (1=USDC, 2=SSV)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x2010357CafE91f56fB0373B02B44b630E3d57836 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 1 2 1000000000000000000000

# SSV → WETH (2=SSV, 0=WETH)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x2010357CafE91f56fB0373B02B44b630E3d57836 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 2 0 10000000000000000000

# WETH → USDC (0=WETH, 1=USDC)
cast send --rpc-url http://57.129.73.144:31133/ --private-key [YOUR_PRIVATE_KEY] 0x2010357CafE91f56fB0373B02B44b630E3d57836 "swap(address,uint8,uint8,uint256)" [YOUR_ADDRESS] 0 1 1000000000000000000000
```

## 🎯 **Expected Flow for this example**

1. **Mint USDC** → Get 1,000,000 USDC
2. **Check USDC** → Verify you have enough
3. **Approve USDC** → Allow swapper to spend USDC
4. **Swap USDC→SSV** → Get ~8,860 SSV
5. **Check SSV** → Verify SSV received
6. **Approve SSV** → Allow swapper to spend SSV
7. **Swap SSV→WETH** → Get ~437.8 WETH
8. **Check WETH** → Verify WETH received
