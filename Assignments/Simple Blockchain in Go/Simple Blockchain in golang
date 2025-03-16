package main

import (
	"crypto/sha256"
	"encoding/hex"
	"fmt"
)

// Block represents a block in the blockchain.
type Block struct {
	transactions []string // List of transactions.
	prevPointer  *Block   // Pointer to the previous block.
	prevHash     string   // Hash of the previous block.
	currentHash  string   // Hash of the current block.
}

// CalculateHash computes the SHA256 hash of a block.
// It concatenates the block's previous hash and all its transactions,
// then returns the hexadecimal representation of the hash.
func CalculateHash(inputBlock *Block) string {
	// Concatenate previous hash and all transactions.
	record := inputBlock.prevHash
	for _, tx := range inputBlock.transactions {
		record += tx
	}
	hash := sha256.Sum256([]byte(record))
	return hex.EncodeToString(hash[:])
}

// InsertBlock creates a new block with the provided transactions,
// links it to the current chain (by setting the prevPointer),
// sets its previous hash appropriately, calculates its current hash,
// and returns the new block as the head of the chain.
func InsertBlock(transactionsToInsert []string, chainHead *Block) *Block {
	newBlock := &Block{
		transactions: transactionsToInsert,
		prevPointer:  chainHead,
	}

	// If there is no existing chain, this is the genesis block.
	if chainHead == nil {
		newBlock.prevHash = "0"
	} else {
		newBlock.prevHash = chainHead.currentHash
	}

	newBlock.currentHash = CalculateHash(newBlock)
	return newBlock
}

// ChangeBlock searches the blockchain for a block that contains the
// transaction equal to oldTrans, changes it to newTrans, and recalculates
// that block's hash. (Note: Subsequent blocks are not updated, so the chain
// integrity will be compromised.)
func ChangeBlock(oldTrans string, newTrans string, chainHead *Block) {
	for block := chainHead; block != nil; block = block.prevPointer {
		for i, tx := range block.transactions {
			if tx == oldTrans {
				block.transactions[i] = newTrans
				// Recalculate the block's hash after modification.
				block.currentHash = CalculateHash(block)
				fmt.Println("Transaction updated in block.")
				return
			}
		}
	}
	fmt.Println("Transaction not found in any block.")
}

// ListBlocks traverses the blockchain from the current head to the genesis block,
// printing out the transactions, current hash, and previous hash for each block.
func ListBlocks(chainHead *Block) {
	index := 0
	for block := chainHead; block != nil; block = block.prevPointer {
		fmt.Printf("Block %d:\n", index)
		fmt.Println("Transactions:")
		for _, tx := range block.transactions {
			fmt.Println(" -", tx)
		}
		fmt.Println("Current Hash:", block.currentHash)
		fmt.Println("Previous Hash:", block.prevHash)
		fmt.Println("--------------------------")
		index++
	}
}

// VerifyChain traverses the blockchain and recalculates the hash for each block.
// It verifies two things:
// 1. The block's stored current hash matches its recalculated hash.
// 2. Each block's stored previous hash matches the actual current hash of the previous block.
// If any inconsistency is found, it prints an appropriate message.
func VerifyChain(chainHead *Block) {
	compromised := false
	for block := chainHead; block != nil; block = block.prevPointer {
		recalculatedHash := CalculateHash(block)
		if block.currentHash != recalculatedHash {
			fmt.Printf("Block with hash %s has been tampered with! (Expected %s)\n",
				block.currentHash, recalculatedHash)
			compromised = true
		}
		// Verify that the stored previous hash matches the actual hash of the previous block.
		if block.prevPointer != nil {
			if block.prevHash != block.prevPointer.currentHash {
				fmt.Printf("Block's previous hash (%s) does not match previous block's current hash (%s).\n",
					block.prevHash, block.prevPointer.currentHash)
				compromised = true
			}
		}
	}
	if compromised {
		fmt.Println("Block chain is compromised.")
	} else {
		fmt.Println("Block chain is unchanged.")
	}
}

func main() {
	var chain *Block

	// Insert blocks into the blockchain. Each new block becomes the new head.
	chain = InsertBlock([]string{"Alice pays Bob 5 BTC", "Charlie pays Dave 3 BTC"}, chain)
	chain = InsertBlock([]string{"Eve pays Frank 2 BTC"}, chain)
	chain = InsertBlock([]string{"Grace pays Heidi 4 BTC"}, chain)

	fmt.Println("Listing blockchain:")
	ListBlocks(chain)

	fmt.Println("\nVerifying blockchain:")
	VerifyChain(chain)

	// Simulate tampering by changing a transaction.
	fmt.Println("\nChanging a transaction to simulate tampering:")
	ChangeBlock("Eve pays Frank 2 BTC", "Eve pays Frank 20 BTC", chain)

	fmt.Println("\nListing blockchain after tampering:")
	ListBlocks(chain)

	fmt.Println("\nVerifying blockchain after tampering:")
	VerifyChain(chain)
}
