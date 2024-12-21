# DeFi Kingdom Clone on Avalanche

This guide walks you through creating a DeFi Kingdom clone on the Avalanche network. You'll set up an EVM-based subnet, define your native currency, and deploy smart contracts to enable gameplay features like battling, exploring, and trading.

---

## 🚀 Project Overview

This project uses Avalanche’s subnet functionality to create a custom EVM chain tailored for gaming and decentralized finance. The native currency can serve as your in-game currency, and you’ll deploy Solidity-based smart contracts to implement the core game logic.

---

## 🛠 Tools Used

- **Unix Computer** (MacOS or Linux)
- **Solidity**: For writing smart contracts.
- **Remix**: Browser-based IDE for Ethereum development.
- **Metamask**: Ethereum wallet and browser extension.
- **Web Browser**

---

## 📚 Setup Guide

### 1. **Set Up Your EVM Subnet**
   - Use the [Avalanche CLI](https://docs.avax.network/) and follow the guide to create a custom EVM subnet.

### 2. **Define Your Native Currency**
   - Customize your EVM subnet to include a native currency. This currency will be used as the in-game currency for your DeFi Kingdom clone.

### 3. **Connect to Metamask**
   - Add your EVM subnet to Metamask by following the steps in the guide. Ensure it is the selected network in Metamask.

### 4. **Deploy Smart Contracts**
   - Use **Remix** and **Metamask** to deploy the building blocks for your game.

---

## 🧩 Core Smart Contracts

### a. **ERC20 Token Contract**
This contract defines your native token, implementing functionality for minting, transferring, and burning tokens.  

#### Example Code:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "Solidity by Example";
    string public symbol = "SOLBYEX";
    uint8 public decimals = 18;

		event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}

```

### b. **Vault Contract**
The Vault contract allows staking and withdrawing tokens, providing essential DeFi functionality for your game.

#### Example Code:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;

    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        /*
        a = amount
        B = balance of token before deposit
        T = total supply
        s = shares to mint

        (T + s) / T = (a + B) / B 

        s = aT / B
        */
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        /*
        a = amount
        B = balance of token before withdraw
        T = total supply
        s = shares to burn

        (T - s) / T = (B - a) / B 

        a = sB / T
        */
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

---

## 🛠 Step-by-Step Instructions

1. **Deploy Your EVM Subnet**  
   - Use the Avalanche CLI to deploy your subnet.

2. **Add Your Subnet to Metamask**  
   - Connect Metamask to your custom subnet.

3. **Connect Remix to Metamask**  
   - Use the Injected Provider in Remix.

4. **Deploy the Smart Contracts**  
   - Deploy the provided `ERC20` and `Vault` contracts.

5. **Test Your Application**  
   - Use Remix to interact with the deployed contracts: mint tokens, stake them in the vault, and more.

---

## 🎮 Gameplay Features

- **Tokens**: Create your in-game currency using the `ERC20` token contract.
- **Staking and Rewards**: Enable staking and reward distribution using the `Vault` contract.
- **DeFi Mechanics**: Use smart contracts to create liquidity pools, enable trading, and incentivize gameplay activities.
---
