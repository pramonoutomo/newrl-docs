---
sidebar_position: 2
slug: /consensus
---

# Newrl Consensus

Newrl uses highly scalable proof of trust (PoT) protocol for consensus
PoT uses the trust scores of participants as the primary input to arrive at consensus in a byzantine fault tolerant manner. It replaces the pure financial stake (as in proof of stake) with a trust score of network participants. This approach enables a richer assessment of the probability of a given node being dishonest. It also creates a stronger disincentive over time for a given node to turn dishonest, because trust scores are harder to earn than to lose. 
Nodes participate in validating transactions and adding blocks - earning platform tokens and a higher probability of future selection for block addition for honest contributions. Conversely, for bad contributions or malicious behaviour, they get a sharp reduction in probability of future selection for block addition and also lose part of their deposited tokens. PoT has superior Byzantine Fault Tolerance because of its two-step confirmation of a block - first by a randomly selected committee and second by the rest of the network. Also, the validation in either case is not just based on simple majority but on trust score weighted assessment.

### Transaction types

The PoT protocol requires the underlying public blockchain to have some architectural specifics. For instance, the blockchain used should be state-based i.e. for validating any transaction, having the latest state is sufficient and each valid transaction updates the state. 
The state can be changed by execution of transactions by each node locally. Transactions are of following types.

1.	Identity management – create new personid and wallet, create a linked wallet etc, signed by KYC custodian
2.	Token management – create a new token, issue more of an existing token, destroy token etc signed by asset custodian / contract manager
3.	Smart contract execution – set up a smart contract from template, deploy it, execute specific functions in it, destroy it (if applicable), signed by contract caller
4.	Two-way token transfer – sender1 sends token1 to sender 2 and sender2 sends token2 to sender1 in an atomic swap - signed by	Sender1 and Sender2
5.	One-way token transfer – sender1 senders token1 to sender 2	- signed by Sender1
6.	Alter trust score – person1 alters the trust score of connection directed from person1 to person2 - signed by Person1
7.	Update network liveness of a node - signed by node owner
8.	Smart contract private state modification, created by smart contract during execution
	
Validation of a transaction is for its signatures as well as economics e.g. for a transfer transaction, the sender needs to have adequate balance.


### Transaction handling

In PoT, any node can broadcast a transaction to its peers. Receiving peers ignore transactions that they already have (checked using transaction id). For valid transactions, each node stores them in the local memory pool and transmits it onwards through a gossip protocol using the transport layer.

### Selection of block creation committee and block minting node

PoT involves block creation by a chosen node and verified by a randomly selected committee , where the selection probability is proportional to its trust score. The minting block is selected randomly inside the committee. Additionally, each node is required to deposit a fixed number of NWRL tokens in a dedicated staking smart contract.

For arriving at consensus, the network undertakes the following steps in each round for a new block creation. The below process begins for a given block immediately upon the inclusion of the previous block.
We assume the following at the end of the state update after the latest block inclusion.

1. Get the eligibler validator list - positive trust score, adequate stake and liveliness check less than 1000 blocks old
2. Select a committee randomly from the eligible validators using previous block hash as the random seed and trust scores as weights
3. Select a validator for current block from within the committee randomly with previous block hash as the random seed. 
Since the state is same for all participants, and the random seed is same, the selection of committee and block creator will be identical for all honest nodes.

### Block creation by the block minting node and broadcast to committee

The minting node is expected to create a block using the below-mentioned transactions.

####	Standard transactions

The node minting a new block will incorporate as many transactions in its local memory pool as fit a single block - ordered in terms of their timestamp. It also transmits these transactions to the committee members, with the expectation that the committee members either already have these transactions or can update their local memory pool with them if they don’t. 

####	Block reward transaction

The block reward transaction in each block creation automatically, with the block creating node as the recipient.

####	Network trust score transaction
The minting node calls the network trust score manager script in the protocol with a selected set of receipts from its memory pool, which refer to previous blocks (not necessarily just the latest one). This script consumes the receipts and updates network trust scores. Each block has zero or more receipts referring to votes on previous blocks. It is further described in the ‘repercussions of voting’ section below.

The transaction fees from all included transactions in the block are transferred to the treasury smart contract. The treasury smart contract either burns NWRL tokens received, or stakes NWRL-NUSD tokens in the liquidity pool of NWRL-NUSD or buys NWRL from the pool with NUSD, based on the relative balance of NWRL and NUSD in its own wallet.

The mining node broadcasts the block to the rest of the committee members with its own signature and receipt as follows.

### Block format
Each block in Newrl has the following format.
1. block_index - An integer
2. proof	- NONCE for the block, set at 0 for all non-empty blocks and 42 for empty ones
3. timestamp	- Time in seconds since the UNIX epoch
4. previous_hash -	Hash of the previous block
5. Status	- Set at 1 for regular block, -1 for empty block with invalid block submission and -2 for empty block with timeouts
6. transactions_hash	- Root hash of the transactions
7. transactions_data	 - {standard transactions}
8. receipts	- {receipts}

### Block receiving and validation by other nodes in the committee
When a committee member receives a block from the minting node, it first validates the block using reference to previous hash, current hash, index etc.


This process leads to creation of a receipt. Block creating node sends a receipt with vote of “9”. Committee members send a vote of 1 for “True” status as per earlier steps above and vote of -2 to indicate that they have insufficient information, or do not want to vote. This is useful in avoiding the other committee members as well as the wider network waiting for a given member’s vote if it wants to abstain. Allowing it to vote -2 reduces the confusion about whether a node is inactive or abstaining.

If the block is found to be invalid, the committee member creates a receipt with vote 5 but transmits an empty block with predetermined characteristics and this vote=5 receipt with it.
Each committee members transmits its receipt to all other committee members and also transmits received receipts to others, in case they haven’t received those from the original sender for whatever reasons. It is expected that at the end of the block_creation_timeout, all live committee members will have a sufficient number of receipts to conclude on block validity.

### New block verification by rest of the network
After committee members broadcast any block as described above to their respective peers and then onwards to the wider network, the block verification and inclusion/rejection by the rest of the network (i.e. other than the committee) is as follows. The first part is to validate the block using receipts alone. This is done as follows.
1.	Validate receipts
- Must be from someone who is a committee member
- Must be signed correctly
2.	Check for the block referred to in the receipt - check if the broadcasted block’s hash matches that in the receipt
3.	Try to validate the block using the received receipts as described earlier above.
4.	If validity status is indeterminate, wait for more receipts.

### Repercussions of voting
Each process of validation of a block by the committee members results in the creation of a vote receipt by that member about the block being valid or not. This receipt has a reference to a block index and the hash of the block being voted on besides the actual vote and the signature of the voter. 

These receipts are broadcast at first by their creator and then onwards like standard transactions by the others in the network i.e. each recipient confirms the validity of the signature and includes in the memory pool as well broadcasts onwards if it is valid. This way, most of the participants in the network have valid receipts of votes by committee members on a given block. While these receipts are used by the committee as well as the rest of the network for validating the current block, they are retained in the memory pool for subsequent processing in network trust score update transaction as follows.

At the time of new block addition, the minting node incorporates as many receipts as possible, with a preference to older receipts, inside the “network trust score update” transaction referred to earlier

