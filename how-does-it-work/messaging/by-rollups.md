# By Rollups

Ethereum ⇌ zkRollups

This cross-chain approach is fundamentally similar to the on-chain light client mechanism, but it leverages the verification mechanisms provided by zkRollups themselves rather than relying on the on-chain light client. For instance, zkSync offers the following methods:

```solidity=
    function proveL2MessageInclusion(
        uint256 _blockNumber,
        uint256 _index,
        L2Message calldata _message,
        bytes32[] calldata _proof
    ) external view returns (bool);
```

> The zkRollup is a Layer 2 scaling solution for Ethereum, with Ethereum's mainnet (Layer 1) serving as its foundation. In a sense, zkRollup also functions as a communication mechanism between Layer 1 and Layer 2. Hence, we leverage its existing message verification mechanisms, as opposed to employing cross-chain communication based on light clients.
