# Unedited learning, WIP

learn with me 

unedited version, just note taking, will consider making an official version if all goes well.

From someone with extremely little knowledge of what's happening under the hood. Also no tech background whatsoever. I audit smart contracts though.

Current understanding level: close to zero
End goal: testing the consensus and execution layer and hoping to find some bugs

What is a consensus?
What is an execution?
What are validators?
What is a node?

How does everything tie into smart contracts?

# Summary 

## Consensus Layer
#### Validators

> There are two primary roles for a validator: 1) checking new blocks and “attesting” to them if they are valid, 2) proposing new blocks when selected at random from the total validator pool. If the validator fails to do either of these tasks when asked they miss out on an ether payout. Validators are also sometimes tasked with signature aggregation and participating in sync committees.

- Validators have 2 main jobs
- Attest to new blocks
- Propose new blocks
- Sometimes: Signature aggregation
- Sometimes: Participate in sync committees

##### Relevant Questions

- How to become a validator?
- Does a validator attest to every block?
- How is a validator chosen to propose new blocks?
- How long can a validator take to attest/propose?
- What does attesting/proposing even mean? How to propose a new block?
- What happens after a block is proposed?
- What happens if a validator doesn't attest or propose?
- How can a validator get paid?

##### Short-form answers

- Deposit 32 ETH
- Not really, depends on which block is chosen (randomly)
- Same, is random
- umm 1 second, 12 seconds and after that have some buffer time, then get penalized
- Written below, have to submit many stuff
- Wait for finalization after 1+ epoch
- Will get penalized
- If he does everything right, attest,propose,sync and on time. More below

##### Random facts

- 1 epoch is 32 slots (~6.4 minutes)
- Checkpoint block (start of every epoch)
- Proposed - Justified - Finalized (total time to finalization ~15 mins (1 epoch after))
- A block is only finalized when it is after a justified block

Interesting to note: notice that finalization takes about 15 minutes but Metamask transactions on ETH takes about 10 seconds-ish? Because these transactions are in the "Justified" stage, they are not finalized yet but more-or-less confirmed. Finalization takes place after 1 epoch, and is counted from checkpoint block to checkpoint block, so about 15 minutes (current epoch - next epoch - next epoch)

##### Proposed Rewards Breakdown

1. source vote: the validator has made a timely vote for the correct source checkpoint
2. target vote: the validator has made a timely vote for the correct target checkpoint
3. head vote: the validator has made a timely vote for the correct head block
4. sync committee reward: the validator has participated in a sync committee
5. proposer reward: the validator has proposed a block in the correct slot

How to get slashed?

1. By proposing and signing two different blocks for the same slot (equivocation)
2. By attesting to a block that "surrounds" another one (effectively changing history)
3. By "double voting" by attesting to two candidates for the same block

Don't understand number 2, but number 1 and 3 is quite straightforward

When attesting, must 



Attestation:

The attestation contains the following components:

1. aggregation_bits: a bitlist of validators where the position maps to the validator index in their committee; the value (0/1) indicates whether the validator signed the data (i.e. whether they are active and agree with the block proposer)
2. data: details relating to the attestation, as defined below
3. signature: a BLS signature that aggregates the signatures of individual validators
The first task for an attesting validator is to build the data. The data contains the following information:

DATA:
1. slot: The slot number that the attestation refers to
2. index: A number that identifies which committee the validator belongs to in a given slot
3. beacon_block_root: Root hash of the block the validator sees at the head of the chain (the result of applying the fork-choice algorithm)
4. source: Part of the finality vote indicating what the validators see as the most recent justified block
5. target: Part of the finality vote indicating what the validators see as the first block in the current epoch

Once the data is built, the validator can flip the bit in aggregation_bits corresponding to their own validator index from 0 to 1 to show that they participated.

Equivocation, Attestation, Epoch, Justified, Finalize:
- https://0xfoobar.substack.com/p/ethereum-proof-of-stake
- https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/attestations/


# Articles

## General Knowledge



## Consensus layer

#### Know more about 
1. https://eth2book.info/capella/


#### Penalties and Rewards calculation
1. [Ethereum Website](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/rewards-and-penalties/)




Some Cool Stuff

##### History
- https://ethereum.org/en/history/#dao-fork-summary