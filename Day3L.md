L means learning

Going to start on this:

https://github.com/ethereum/consensus-specs/blob/9f643d8dbf678430bc6707f0e5b914d0bf62545c/specs/phase0/beacon-chain.md

Still don't know how the consensus stuff comes into play, eg lighthouse prysm, and how it all works together, but now a better understanding of how validators work on a high level.

Day3 try to understand the lower level of beacon chain and how all the other consensus clients interact with it later.

I don't get it, looking around consensus-specs, where is the actual code? How does it relate to the consensus clients?? How can the test run?

Trying to run the test, but where are all the functions from?

Like in this:

https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/fork-choice.md#extended-payloadattributes

It says:

Extended PayloadAttributes

PayloadAttributes is extended with the parent beacon block root for EIP-4788.

When i find class PayloadAttributes(object), where is the actual python code? I can only find md files

Or like def on_block

Where is the md file getting all these python code? Or are they just references?

I'm actually lost

Ok seems like they're all in the test folder

attestation, block, etc etc

The next question is how does it relate to the consensus clients? Do they rewrite it in different language?

Found something similar

https://github.com/ethereum/consensus-specs/blob/dev/presets/mainnet/phase0.yaml

Eth specs and Lodestar

https://github.com/ChainSafe/lodestar/blob/ad8c10e7d9fd3b66c2e49133fcb2b2a5f7846c6d/packages/params/src/presets/mainnet.ts#L66

Reading eth2book.info again now

Validator lifecycle

1. A 32 ETH deposit has been made on the Ethereum 1 chain. No validator record exists yet.
2. The deposit is processed by the beacon chain at some slot. A validator record is created with all epoch fields set to FAR_FUTURE_EPOCH.
3. At the end of the current epoch, the activation_eligibility_epoch is set to the next epoch.
4. After the epoch activation_eligibility_epoch has been finalised, the validator is added to the activation queue by setting its activation_epoch appropriately, taking into account the per-epoch churn limit and MAX_SEED_LOOKAHEAD.
5. On reaching activation_epoch the validator becomes active, and should carry out its duties.
6. At any time after SHARD_COMMITTEE_PERIOD epochs have passed, a validator may request a voluntary exit. exit_epoch is set according to the validator's position in the exit queue and MAX_SEED_LOOKAHEAD, and withdrawable_epoch is set MIN_VALIDATOR_WITHDRAWABILITY_DELAY epochs after that.
7. From exit_epoch onward the validator is no longer active. There is no mechanism for exited validators to rejoin: exiting is permanent.
8. After withdrawable_epoch, the validator's full stake can be withdrawn.

Compare:
Text:https://eth2book.info/capella/part3/containers/blocks/
Block:https://beaconcha.in/slot/10773164

Some DoS??
https://github.com/ethereum/consensus-specs/issues/1339

To watch:

https://www.youtube.com/watch?v=V0RjGmFE35U
https://www.youtube.com/watch?v=X4eiaH0wy1w