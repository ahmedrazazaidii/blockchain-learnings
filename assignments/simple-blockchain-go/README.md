# Simple Blockchain in Go

This repository contains a basic blockchain implementation written in Go. It demonstrates key blockchain concepts such as block creation, linking blocks using SHA256 hashing, and verifying chain integrity.

## Features

- **Block Structure:** Each block contains a list of transactions, a pointer to the previous block, the previous block's hash, and its own hash.
- **Hash Calculation:** Uses SHA256 to compute each block's hash based on its transactions and the previous hash.
- **Block Insertion:** New blocks are added to the chain, maintaining the linkage by storing the hash of the previous block.
- **Tampering Simulation:** A function to modify a transaction is provided, demonstrating how tampering affects the chain.
- **Verification:** The blockchain can be verified by recalculating hashes and checking that the chain's integrity is maintained.

## Getting Started

### Prerequisites

- [Go](https://golang.org/dl/) must be installed on your machine.
