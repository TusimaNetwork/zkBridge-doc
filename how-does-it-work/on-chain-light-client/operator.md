# Operator

The Operator is an off-chain role that, in collaboration with the **smart contract** and **circuit**, constitutes the On-chain Light Client. It serves as an intermediary between the two and plays a critical role in ensuring the timely update of the latest block header information. The Operator's primary functions are as follows:

1. Running the `VerifyHeader` circuit and submitting the resulting proof and block header information to the `updateHeader` function within the smart contract.
2. Executing the `VerifySyncCommittee` circuit and submitting the generated proof along with the sync committee member list to the `updateSyncCommittee` function in the smart contract.
