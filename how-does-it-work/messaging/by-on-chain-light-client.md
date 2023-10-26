# By On-chain Light Client

![](https://ucarecdn.com/9405ab91-7085-4315-ad62-0bff892f20d1/messageverification.png)

In Ethereum's blockchain, Merkle tree structures are widely used, making the process of **verification** relatively straightforward. In the Merkle tree verification process, validators don't need to possess all the node data within the Merkle tree. Instead, they only need to know the Merkle root. Provers can prove the existence of a specific node by sending the validator the node data to be proven, along with the corresponding branch data (referred to as the Merkle proof).

When a user transmits a message (MSG) through the Messaging contract, that message is stored in a specific memory slot within the contract. All the memory slots in the contract are serialized into a Merkle tree structure. The root of this Merkle tree is referred to as the `storage root`. In Ethereum blocks, multiple storage roots are combined to form another Merkle tree, and its root is called the `account root`. Several `account roots`, in turn, make up the `execution state root`, and this hierarchy continues until it reaches the `body root` included in the block header. Fortunately, the `body root` is provided by the on-chain light client and has been verified to be correct.

The message that we transmit is stored in one of the leaf nodes of the Merkle tree with the root being the `storage root`. When we need to prove to the destination chain that we indeed sent a message, such as "Hello world," from the source chain, all we have to send to the destination chain is the message itself and a series of Merkle proofs corresponding to it.

This is the essence of what Messaging does. Messaging consists of two main components:

- Contract: This part is responsible for verifying messages and forwarding them to the target contract.
- Relayer: The relayer is in charge of sending the message body to the contract and validating the necessary `Merkle proofs` for message verification.
