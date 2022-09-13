---
sidebar_position: 2
slug: /consensus
---

# Newrl Consensus

Newrl uses highly scalable proof of trust (PoT) protocol for consensus
PoT uses the trust scores of participants as the primary input to arrive at consensus in a byzantine fault tolerant manner. It replaces the pure financial stake (as in proof of stake) with a trust score of network participants. This approach enables a richer assessment of the probability of a given node being dishonest. It also creates a stronger disincentive over time for a given node to turn dishonest, because trust scores are harder to earn than to lose. 
Nodes participate in validating transactions and adding blocks - earning platform tokens and a higher probability of future selection for block addition for honest contributions. Conversely, for bad contributions or malicious behaviour, they get a sharp reduction in probability of future selection for block addition and also lose part of their deposited tokens. PoT has superior Byzantine Fault Tolerance because of its two-step confirmation of a block - first by a randomly selected committee and second by the rest of the network. Also, the validation in either case is not just based on simple majority but on trust score weighted assessment.

## Overall Architecture for Implementing Proof of Trust

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
	

A transaction in Newrl has the standard format as below.
{“transaction”: “data”, 
“signatures”: [{sign1}, {sign2}]}

Validation of a transaction is for its signatures as well as economics e.g. for a transfer transaction, the sender needs to have adequate balance.

## Protocol functioning
There are three types of computations carried out in a typical public blockchain - transaction validation, block creation and verification of a block upon its broadcast and reception by other nodes.

### Transaction handling

In PoT, any node can broadcast a transaction to its peers. Receiving peers ignore transactions that they already have (checked using transaction id). For valid transactions, each node stores them in the local memory pool and transmits it onwards through a gossip protocol using the transport layer.

### Selection of block creation committee and block minting node

PoT involves block creation by a chosen node and verified by a randomly selected committee , where the selection probability is proportional to its trust score. The minting block is selected randomly inside the committee. Additionally, each node is required to deposit a fixed number of NWRL tokens in a dedicated staking smart contract.
For arriving at consensus, the network undertakes the following steps in each round for a new block creation. The below process begins for a given block immediately upon the inclusion of the previous block.
We assume the following at the end of the state update after the latest block inclusion.
N = Number of total validator nodes in the network (from the nodes table)
si = Network trust score of the ith node
C = Size of the block creation committee
The selection space S for block creation group is created is follows.
    〖node〗_i∈S if s_i≥0 and 〖stake〗_i≥〖stake〗_minimum
    S=sort(S,ascending,peerid)
Since the state is same for all participants, S will be identical for them.
We select a total of C nodes in the committee from S.
    S_1=S
    d_1= random(seed=〖hash〗_(latest_block ))×size(S_1)
    〖node〗_j=node at d_j^th place in S_j
     S_j=S_(j-1)-〖node〗_j
    d_j=random(seed=d_(j-1))×size(S_j)
When j = C, the selection process halts. C nodes are now selected for the committee.
The block minting node is always selected from the committee itself. Since everyone is starting with the same state and using the same hash as the first seed, the choices of all committee members as well as the minting node for a block will be same for all.
Block creation by the minting node and broadcast to committee
The minting node is expected to create a block using the below-mentioned transactions.
	Standard transactions
The node minting a new block will incorporate as many transactions in its local memory pool as fit a single block - ordered in terms of their timestamp. It also transmits these transactions to the committee members, with the expectation that the committee members either already have these transactions or can update their local memory pool with them if they don’t. 
	Block reward transaction
The block reward transaction in each block creation automatically, with the block creating node as the recipient.
	Network trust score transaction
The minting node calls the network trust score manager script in the protocol with a selected set of receipts from its memory pool, which refer to previous blocks (not necessarily just the latest one). This script consumes the receipts and updates network trust scores. Each block has zero or more receipts referring to votes on previous blocks. It is further described in the ‘repercussions of voting’ section below.
The transaction fees from all included transactions in the block are transferred to the treasury smart contract. The treasury smart contract either burns NWRL tokens received, or stakes NWRL-NUSD tokens in the liquidity pool of NWRL-NUSD or buys NWRL from the pool with NUSD, based on the relative balance of NWRL and NUSD in its own wallet.

The mining node broadcasts the block to the rest of the committee members with its own signature and receipt as follows.

### Block format
Each block in Newrl has the following format.
block_index	An integer
proof	NONCE for the block, set at 0 for all non-empty blocks and 42 for empty ones
timestamp	Time in seconds since the UNIX epoch
previous_hash	Hash of the previous block
Status	Set at 1 for regular block, -1 for empty block with invalid block submission and -2 for empty block with timeouts
transactions_hash	Root hash of the transactions
transactions_data	{standard transactions}
receipts	{receipts}

Block receiving and validation by other nodes in the committee
When a committee member receives a block from the minting node, it first validates the block as shown in the box below.


This process leads to creation of a receipt, which looks as follows.
{“receipt_data”: {“block_index”:<index>, 
“block_hash”: <hash>, 
“vote”: 1, 9, 5 or -2}, 
“address”:<address>, 
“signature”:<signature>}


Block creating node sends a receipt with vote of “9”. Committee members send a vote of 1 for “True” status as per earlier steps above and vote of -2 to indicate that they have insufficient information, or do not want to vote. This is useful in avoiding the other committee members as well as the wider network waiting for a given member’s vote if it wants to abstain. Allowing it to vote -2 reduces the confusion about whether a node is inactive or abstaining.
If the block is found to be invalid, the committee member creates a receipt with vote 5 but transmits an empty block with predetermined characteristics and this vote=5 receipt with it.
Each committee members transmits its receipt to all other committee members and also transmits received receipts to others, in case they haven’t received those from the original sender for whatever reasons. It is expected that at the end of the block_creation_timeout, all live committee members will have a sufficient number of receipts to conclude on block validity.
