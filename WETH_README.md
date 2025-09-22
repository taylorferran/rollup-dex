# WETH (Wrapped Ether) Contract

## Overview

This WETH contract provides a standard ERC-20 interface for wrapping and unwrapping native ETH on both rollup networks. It includes additional `burn` and `mint` functions for bridging purposes.

## Contract Details

- **Contract Address**: `0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553`
- **Name**: Wrapped Ether
- **Symbol**: WETH
- **Decimals**: 18
- **Solidity Version**: 0.7.6

## Network Information

### Rollup A
- **Chain ID**: 77777
- **RPC URL**: http://57.129.73.156:31130/
- **Contract Address**: `0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553`

### Rollup B
- **Chain ID**: 88888
- **RPC URL**: http://57.129.73.144:31133/
- **Contract Address**: `0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553`

## Contract Functions

### Standard ERC-20 Functions
- `totalSupply()` - Returns total supply of WETH
- `balanceOf(address account)` - Returns WETH balance of account
- `transfer(address to, uint256 amount)` - Transfers WETH to another address
- `approve(address spender, uint256 amount)` - Approves spender to transfer WETH
- `allowance(address owner, address spender)` - Returns allowance amount
- `transferFrom(address from, address to, uint256 amount)` - Transfers WETH on behalf of owner

### Wrap/Unwrap Functions
- `deposit()` - Wraps ETH into WETH (payable function)
- `withdraw(uint256 wad)` - Unwraps WETH back to ETH
- `receive()` - Automatically wraps ETH sent to contract

### Bridge Functions
- `burn(address account, uint256 value)` - Burns WETH from account (for bridging out)
- `mint(address account, uint256 value)` - Mints WETH to account (for bridging in)

## Usage Examples

### 1. Wrap ETH to WETH

```bash
# Using cast (from Foundry)
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "deposit()" --value 1ether --rpc-url http://57.129.73.156:31130/ --private-key YOUR_PRIV_KEY
```

### 2. Unwrap WETH to ETH

```bash
# Withdraw 0.5 ETH worth of WETH
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "withdraw(uint256)" 500000000000000000 --rpc-url http://57.129.73.156:31130/ --private-key YOUR_PRIV_KEY
```

### 3. Check WETH Balance

```bash
# Check balance of an address
cast call 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "balanceOf(address)" 0x4da9f34f83d608cAB03868662e93c96Bc9793495 --rpc-url http://57.129.73.156:31130/
```

### 4. Transfer WETH

```bash
# Transfer 0.1 WETH to another address
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "transfer(address,uint256)" 0x1234567890123456789012345678901234567890 100000000000000000 --rpc-url http://57.129.73.156:31130/ --private-key YOUR_PRIV_KEY
```

### 5. Burn WETH (for bridging out)

```bash
# Burn 0.25 WETH from an account
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "burn(address,uint256)" 0x4da9f34f83d608cAB03868662e93c96Bc9793495 250000000000000000 --rpc-url http://57.129.73.156:31130/ --private-key YOUR_PRIV_KEY
```

### 6. Mint WETH (for bridging in)

```bash
# Mint 0.1 WETH to an account
cast send 0x356dA0CBA100a69B3FD3F2Ce4871B7e3921E7553 "mint(address,uint256)" 0x4da9f34f83d608cAB03868662e93c96Bc9793495 100000000000000000 --rpc-url http://57.129.73.156:31130/ --private-key YOUR_PRIV_KEY
```

## Deployment Information

The contract was deployed using CREATE2 to ensure the same address on both rollups:

- **Salt**: `0x1234567890123456789012345678901234567890123456789012345678901234`
- **Deployer**: `0x4da9f34f83d608cAB03868662e93c96Bc9793495`
- **Deployment Script**: `script/DeployWETH.s.sol`

## Events

The contract emits the following events:

- `Transfer(address indexed from, address indexed to, uint256 value)` - Standard ERC-20 transfer event
- `Approval(address indexed owner, address indexed spender, uint256 value)` - Standard ERC-20 approval event
- `Deposit(address indexed dst, uint256 wad)` - Emitted when ETH is wrapped
- `Withdrawal(address indexed src, uint256 wad)` - Emitted when WETH is unwrapped
