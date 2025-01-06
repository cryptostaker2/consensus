From https://www.youtube.com/watch?v=V0RjGmFE35U

What is GHOST and Casper

On E2.0, a set of validators want to form a consensus history
- validator broadcast messages (blocks and attestations) over the network
- Each validator has their own view (possibly incomplete history) of which messgages they "saw"

The view problem
- views can be out of sync because of latency

1. Need a finality view to tell which blocks are "final"
- safety: validators never finalize conflicting (forked) blocks
- liveness: validator continne to create and finalize new blocks
  
2. A fork-choice rule that tells validator where to build blocks
- a function takes a view as input and outputs acanonical chain ending at a leaf block (the head of the chain)

eg Longest Chain Rule in Bitcoin

These properties imply we can grow a chain of finalized blocks as our "consensus history"

Fork-choice rule -> **GHOST (Greediest Heaviest Observed Subtree)**
**LMD Ghost (Latest Message Driven GHOST)** fork choice rule tells validators to follow the fork with the most "latest messages" (attesttations) attached to the blocks

Eg 7 validators
7-7-7-2-2-1
7-7-7-5-0
7-7-7-5-4
7-7-7-1

Go down the one with the most number of attestations (no more the longest chain)
Same goes for attesting and proposing

Toy Protocol to start
- slots are the basic unit of time; each slots is assigned a single validator as block proposer
- an epoch is a length of slots, during which each validator attests once
- each validator is assigned to 1 slot per epoch as an attestor
- beginning of each slot, the block proposer makes a block
- middle of each slot, the attestor validators attest (with LMD Ghost rule)

NOTE: this video is 4 years old, so now an epoch length is 32 (not 64) and a block is 12 seconds (not 6)

Introducing Gasper
- add the finalization rule (Casper FFG) to fork choice rule (LMD GHOST)

LMD Ghost
- only tells you the best chain to go down
- doesn't tell you which blocks is final

**Casper FFG (Friendly Finality Gadget)** is a consensus "finalization gadget" that can be used on top a blockchain
- must already have a blockchain, define certain things as being finalized. Pick some of the blocks so that they form a chain
- Introduces the idea of "checkpoints" on the blockchain. Special blocks (sometimes purple (final), sometimes not)

Some blocks can serves as checkpoints to the chain
- FFG attestation is a vote from checkpoint block A to a checkpoint block B
- A checkpoint block is justified if more than 2/3 of attestations in thie epoch vote from checkpoints A to B
- If B is justified and an adjacent checkpoint C is justified by B, then B is finalized

From someone else to you, and you to someone else, then you become finalized.

Slashing conditions
- Slashing conditions provide a financial incentive for validators to follow the protocol. FFG defines the following slashing conditions:
- No validator makes two distinct attestations in the same epoch
- No validator makes two tattestations to blocks within the span of another epoch. Cannot attest two pairs of transactions

Safety and Liveness
- given any view G, if finalized pair conflict in F, then G has enough evidence to slash at least 1/3 of the validators.
- if at least 2N/3 stake worth of the validators are honest, then it is always possible for a new block to be finalized with the honest validators continuing to follow the protocol, no matter what happened previously to the blockchain

Probabilistic Liveness
- supposed people are honest, we should plausibly be able to make new blocks
- equivocation game rules

How it works in one epoch:
- we have some honest validators and some dishonest validators 2:1
- There are 2 options, blocks A and B
- The game takes place over 1 slot unit of time
- At time 0.5, all the honest validators attest for the option that so far has a majority of votes
- Bad guys -> nothing gets 2/3 of the votes, nothing gets justified and finalized
- Honest validators win if either choice gets more than 2/3 of the votes. Dishonest validators win if neither choice does.
- winning is at the extremes and losing is at the middle

Eg If the dishonest validators can "vote earlier" with the network latency = 0.3
Dishonest validators vote at time 0.2
Honest validators vote at time 0.5
Now the honest validators don't know which one to vote for, have to flip a coin.

Justification Liveness therorem
At the first slot, some blocks come out ahead 
*****

Finalization Liveness

A Combinatorics problem

Summary of proofs for probabilistic liveness
1. In an epoch, a block in the first slot is likely to get a lot of attestations with probability r
2. If r is high, then the block is likely to be justified in this epoch with some probability p.
3. If p is high, it is likely for a block to be finalized in a sequence of n epoch, as n gets large.


Good resources

The future of upgrades:
https://www.galaxy.com/insights/research/pectra-upgrade-and-other-eth-catalysts/#:~:text=By%20the%20end%20of%20August,even%20more%20to%20this%20list.

More context on Gasper:
https://www.galaxy.com/insights/research/paths-toward-reducing-validator-set-size-growth/

I didn't know we are already in Deneb + Cancun, and Electra + Prague and Fulu + Osaka is already in the works.

I will be learning about the changes from phase0 onwards, and walk through all the EIPS

No wonder my Remix keeps saying Remix VM(Cancun) 

Interesting Fact: We are currently at ~1M+ validators, and apparently it's quite bad? and VB states the network can handle 4M validators, apparently EIP7251 in pectra will change this.

Also wanted to know why so many validators is a bad thing and what they are going to do about it.

Apparently the effecive balance is going from 32 ETH to 2048? lol I don't even have I ETH.