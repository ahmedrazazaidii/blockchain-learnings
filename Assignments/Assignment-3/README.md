# Election DApp

A simple Ethereum-based voting application (dApp) built using Solidity, Truffle, Web3.js, and Bootstrap. This project partially follows the [Dapp University "Ultimate Ethereum Dapp Tutorial"](https://www.dappuniversity.com/articles/the-ultimate-ethereum-dapp-tutorial).

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Prerequisites](#prerequisites)  
3. [Installation & Setup](#installation--setup)  
4. [Contract Development](#contract-development)  
5. [Testing](#testing)  
6. [Frontend Integration](#frontend-integration)  
7. [Manual Wallet Connection](#manual-wallet-connection)  
8. [Running the Application](#running-the-application)  

---

## Project Overview

This Election DApp allows users to vote for one of two candidates. It includes:

- **Solidity Smart Contract** (`Election.sol`) that:  
  - Models candidates and vote counts  
  - Tracks voter addresses to prevent double-voting  
  - Emits events when a vote is cast  
- **Truffle Tests** (`test/election.js`) to verify contract behavior  
- **Web Frontend** (`index.html`, `app.js`) using Web3.js for blockchain interaction  
- **Manual "Connect Wallet"** flow for MetaMask integration  

---

## Prerequisites

- Curl
- Node.js & npm 
- Truffle (`npm install -g truffle`)  
- Ganache (local Ethereum blockchain)  
- MetaMask browser extension  
- Git & GitHub account (optional, for version control)  

---

## Installation & Setup

1. **Clone the repo**  
   ```bash
   git clone https://github.com/ahmedrazazaidii/blockchain-learnings.git
   cd blockchain-learnings/Assignments/Assignment-3/election-dapp
   ```

2. **Install dependencies**  
   ```bash
   npm install
   ```

3. **Start Ganache**  
   - Open Ganache GUI or use ganache cli and spin up a local blockchain on `http://127.0.0.1:7545`
   - Add a new workspace in Ganache and give a name to the workspace
   - Add file `truffle-config.js` in that workspace and start

4. **Compile & migrate contracts**  
   ```bash
   truffle compile
   truffle migrate --reset --network development
   ```

---

## Contract Development

- **Path:** `contracts/Election.sol`  
- **Key elements:**
  - `struct Candidate { uint id; string name; uint voteCount; }`
  - `mapping(address => bool) public voters` to prevent double votes  
  - `mapping(uint => Candidate) public candidates` to store candidates  
  - `constructor` adds two initial candidates  
  - `vote(uint _candidateId)` function with `require` checks and `votedEvent` emission

---

## Testing

- **Path:** `test/election.js`  
- **Setup:** Deploy a fresh contract in `beforeEach()` using `Election.new()` to isolate tests  
- **Tests include:**  
  1. Initialization with two candidates  
  2. Correct candidate values (ID, name, voteCount)  
  3. Casting a valid vote, checking event and state change  
  4. Revert on invalid candidate ID  
  5. Revert on double voting

- **Run tests:**  
  ```bash
  truffle test
  ```

---

## Frontend Integration

1. **Files**:  
   - `src/index.html` — main UI with Bootstrap styling  
   - `src/js/app.js` — Web3.js logic and TruffleContract setup  

2. **Key functions in `app.js`:**  
   - `initWeb3()` — detect MetaMask (`window.ethereum`) or fallback  
   - `bindEvents()` — attach handler to "Connect Wallet" button  
   - `connectWallet()` — calls `eth_requestAccounts`, displays address  
   - `render()` — fetches `candidatesCount`, populates table and `<select>`, checks `voters(account)` to hide form if already voted  
   - `castVote()` — sends `vote()` transaction and shows loader until confirmation

---

## Manual Wallet Connection

Rather than auto-requesting on load, we added a **Connect Wallet** button:

1. Button placed outside the hidden content area:  
   ```html
   <button id="connectWallet" class="btn btn-secondary">
     Connect Wallet
   </button>
   ```

2. In `app.js`, `connectWallet()` calls:  
   ```js
   const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
   App.account = accounts[0];
   document.getElementById('accountAddress').innerText = 'Connected: ' + App.account;
   App.render();
   ```

This ensures MetaMask prompts only when the user clicks, and the UI doesn’t show vote options until after connecting.

---

## Running the Application

```bash
# 1. Start local blockchain
ganache-cli

# 2. In a second terminal, run migrations (if you’ve updated contracts)
truffle migrate --reset --network development

# 3. Start front-end server
npm run dev

# 4. Open http://localhost:5173 (or your dev server URL) in browser
# 5. Click 'Connect Wallet', and connect to the MetaMask account
# 6. View candidates and cast your vote
```

---

Feel free to modify and extend the Election DApp for further endeavours.

