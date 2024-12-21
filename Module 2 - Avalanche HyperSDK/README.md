# Building a Blockchain with HyperSDK: TokenVM

This README provides an overview of building a custom blockchain using **HyperSDK** and the **TokenVM** as an example. The TokenVM showcases the creation, minting, transferring, and trading of tokens using the HyperSDK framework, offering a powerful way to build and deploy custom virtual machines tailored to specific needs.

---

## 📖 Project Introduction

The challenge:  
Create a **custom virtual machine** that enables users to mint, transfer tokens, and trade them. HyperSDK simplifies this by providing the tools needed to define the rules and functionality of your blockchain, including token management and order book-based trading.

---

## 🤔 What is TokenVM?

TokenVM is an example virtual machine created by the **Metacrafters** team to demonstrate the use of HyperSDK for tasks such as:

- Token minting
- Metadata updates
- Asset burning
- On-chain trading with an embedded order book

The TokenVM includes a powerful CLI tool for easy interaction and supports RPC requests for trades using an in-memory order book.

---

## 🚀 Features of TokenVM

### 1. **Arbitrary Token Minting**
TokenVM allows users to create custom tokens, mint new ones, transfer them, and update metadata. Features include:
- Native token support with efficient storage:  
  Each balance entry requires only **72 bytes** of state.
- Parallel transaction execution for transfers that don’t involve the same accounts.

---

### 2. **On-Chain Trading**
TokenVM enables fully on-chain trading by supporting:
- Creation of trade offers specifying token rates.  
- Filling of partial or complete offers.  
- An **in-memory order book** for efficient trade matching.

Orders require **152 bytes** of state and allow parallel execution of fills, even within the same trading pair.

---

### 3. **In-Memory Order Book**
The in-memory order book listens for on-chain orders and maintains their state, ensuring efficient trade execution. Features:
- Organized as a max heap per pair, prioritizing the best rates.
- Uses HyperSDK’s transaction feed for real-time updates.

---

## 🛠 Steps to Reproduce

### Initial Setup
1. **Clone the Repository**  
   ```bash
   git clone <repository-url>
   ```
2. **Normalize Dependencies**  
   ```bash
   go mod tidy
   ```
3. **Configure Constants**  
   Update the constants in `consts/consts.go`.

4. **Register Actions**  
   Add `Create_Asset` and `Mint_Asset` actions in `registry/registry.go`.

5. **Run the VM Locally**  
   - Ensure **Go** is on your system path:
     ```bash
     export PATH=$PATH:$(go env GOPATH)/bin
     ```
     Or:
     ```bash
     export PATH=$PATH:/usr/local/go/bin
     ```
   - Execute the run script:  
     ```bash
     MODE="run-single" ./scripts/run.sh
     ```
   - Build the project:  
     ```bash
     ./scripts/build.sh
     ```
   - If permission is denied, run scripts with `bash`:  
     ```bash
     bash ./scripts/run.sh
     ```

6. **Load Demo Keys**  
   ```bash
   ./build/token-cli key import demo.pk
   ./build/token-cli chain import-anr
   ```

---

## 🎮 Using TokenVM

### 1. **Creating & Minting Tokens**
#### Step 1: Create an Asset  
   ```bash
   ./build/token-cli action create-asset
   ```
   Example output:  
   ```
   metadata: MarioCoin
   ✅ txID: 27grFs9v...
   ```

#### Step 2: Mint Tokens  
   ```bash
   ./build/token-cli action mint-asset
   ```
   Example output:  
   ```
   metadata: MarioCoin
   amount: 10000
   ✅ txID: X1E5CVF...
   ```

#### Step 3: Check Balance  
   ```bash
   ./build/token-cli key balance
   ```
   Example output:  
   ```
   balance: 10000 MarioCoin
   ```

---

### 2. **Trading Tokens**
#### Create a Trade Offer
Submit an on-chain trade offer by creating an "export" action:  
   ```bash
   ./build/token-cli action export
   ```

#### Import on the Destination Chain
The export command automatically triggers an import on the destination. Example:  
   ```
   txID: 24Y2zR...
   ✔ switch default chain to destination (y/n): y
   ```

---

## 🧩 Conclusion

By using **HyperSDK** and **TokenVM**, you can easily create a custom blockchain for minting and trading tokens. The efficient storage format, parallel transaction execution, and in-memory order book make it ideal for scalable applications. Follow the steps above to deploy and interact with your TokenVM blockchain!
