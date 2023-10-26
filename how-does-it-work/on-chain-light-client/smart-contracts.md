# Smart contracts

The smart contract represents the ultimate manifestation of the On-chain Light Client, as it is deployed on the destination chain. It is designed to provide external access to source chain block header information, allowing any developer to read this information for the purpose of verifying transactions or events occurring on the source chain.

The smart contract features two primary write functions, which are called by the Operator:
- `updateHeader`, used for updating the source chain block headers
    ```solidity=
        /// @notice Updates the execution state root given a finalized light client update
        /// @dev The primary conditions for this are:
        ///      1) At least 2n/3+1 signatures from the current sync committee where n = 512
        ///      2) A valid merkle proof for the finalized header inside the currently attested header
        /// @param update a parameter just like in doxygen (must be followed by parameter name)
        function updateHeader(HeaderUpdate calldata update) external
    ```
    
    This function mainly does two tasks:
    1. Verifying the BLS signature, that is, verifying proofs generated in the circuit.
    2. Updating the block header after the BLS signature was successfully verified.
    
- `updateSyncCommittee`, used for routine update of the sync committee member information, with updates occurring approximately once daily.
    ```solidity=
        /// @notice Update the sync committee, it contains two updates actually: 
        ///         1. syncCommitteePoseidon
        ///         2. a header
        /// @dev Set the sync committee validator set root for the next sync committee period. This root 
        ///      is signed by the current sync committee. To make the proving cost of _headerBLSVerify(..) 
        ///      cheaper, we map the ssz merkle root of the validators to a poseidon merkle root (a zk-friendly 
        ///      hash function)
        /// @param update The header
        /// @param nextSyncCommitteePoseidon the syncCommitteePoseidon in the next sync committee period
        /// @param commitmentMappingProof A zkSnark proof to prove that `nextSyncCommitteePoseidon` is correct
        function updateSyncCommittee(
            HeaderUpdate calldata update,
            bytes32 nextSyncCommitteePoseidon,
            Groth16Proof calldata commitmentMappingProof
        ) external
    ```
    
    This function serves two essential purposes:
    1. Simultaneously updating the block headers for the current period since the sync committee roster is embedded within the block headers.
    3. Updating the sync committee roster.
    
    Given that the members of the sync committee for each period are determined in the preceding period and included in the block headers of that period, a light client must periodically save the roster for the next period within the contract. This is used for verifying BLS signatures to confirm whether validator public keys have been tampered with.

    > As the sync committee comprises 512 members, storing all of them in the smart contract would incur a significant gas cost. In practice, it's sufficient to store the hash values of the 512 validator public keys. For detailed logic, please refer to the **Circuits section**.
