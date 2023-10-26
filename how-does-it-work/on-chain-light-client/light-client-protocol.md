# Light client protocol

![](https://ucarecdn.com/875914d5-e930-41c0-a17b-2da59d6c50cc/lightsync.png)

The following is the definition of the Light Client Protocol as outlined in the [Ethereum official specifications](https://github.com/ethereum/annotated-spec/blob/master/altair/sync-protocol.md#introduction):

> The sync committee is the "flagship feature" of the Altair hard fork. This is a committee of 512 validators that is randomly selected every sync committee period (~1 day), and while a validator is part of the currently active sync committee they are expected to continually sign the block header that is the new head of the chain at each slot.
>
> The purpose of the sync committee is to allow light clients to keep track of the chain of beacon block headers. The other two duties that involve signing block headers, block proposal and block attestation, do not work for this function because computing the proposer or attesters at a given slot requires a calculation on the entire active validator set, which light clients do not have access to (if they did, they would not be light!). Sync committees, on the other hand, are (i) updated infrequently, and (ii) saved directly in the beacon state, allowing light clients to verify the sync committee with a Merkle branch from a block header that they already know about, and use the public keys in the sync committee to directly authenticate signatures of more recent blocks.

From this explanation, it becomes evident that the core of the Ethereum Light Client Protocol is the sync committee, comprising 512 dynamically rotating validators. The rotation rule involves a change in membership every epoch, with the validator slots for each epoch being pre-determined in the preceding one. This means that within each epoch, every block contains the validator slots for the next epoch, which is crucial for checking whether the public key used for signatures originates from a genuine validator.

For an Ethereum block to be deemed valid, it must receive signatures from over two-thirds of the sync committee members. As a result, a light client needs to verify if over two-thirds of validators have signed a block header; otherwise, the block header is considered invalid.

Due to the Ethereum Light Client Protocol's incorporation of complex computational logic, such as the use of the Curve BLS12-381 for signature algorithms within the sync committee, the on-chain operational costs are exceptionally high, often surpassing the gas limit per block. To address this challenge, zkSNARKs technology is leveraged to convert intricate logic into zkSNARKs' circuit programs. The generated proofs are then submitted to on-chain smart contracts for verification. Since the time required for proof verification is significantly less than the execution time for complex logic, this approach effectively reduces the gas resources consumed by smart contracts on-chain, making it feasible to implement the Ethereum Light Client Protocol on the blockchain.


> zk-SNARK allows you to generate a proof that some computation has some particular output, in such a way that the proof can be verified extremely quickly even if the underlying computation takes a very long time to run. For additional information about zkSNARKs, please click [here](https://vitalik.ca/general/2021/01/26/snarks.html).

The on-chain light client consists of three key components: the contract, the circuit, and the operator.
