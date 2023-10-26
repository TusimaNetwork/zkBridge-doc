# Relayer

The `Relayer` corresponds to the `IRelayer` section discussed in the previous section-[Smart contracts](smart-contracts.md). It is responsible for forwarding messages from the source chain to the destination chain by calling the `IRelayer` interface.

The `Relayer`'s sole responsibility is to transmit messages and provide Merkle Proofs that verify the authenticity of the messages. The Merkle Proof is verified within the smart contract. Messages forwarded by honest `Relayer` are accepted, while those sent by malicious `Relayer` are rejected.

Once the messages are successfully verified within the smart contract, they are passed through the `IReceiver` interface to the smart contract provided by the user. The messages received by the user have been proven to be genuine and reliable.

```solidity
interface IReceiver {
    function handleMsg(
        uint32 sourceChainId,
        address sourceAddr,
        bytes memory message
    ) external returns (bytes4);
}
```
