---
description: Tusima zkBridge Messaging Smart contracts
---

# Smart contracts

The `Messaging` smart contract is the communication protocol used by devs to facilitate communication between different chains. It is deployed on both the source and destination chains and is responsible for handling message transmission and reception.

The Messaging component of Tusima zkBridge provides developers with the `IMessaging.sol` interface file, which roughly includes the following:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

enum MessageStatus {
    // ...
}

struct Message {
    // ...
}

interface ISender {
    // ...
}

interface IRelayer {
    // ...
}

interface IReceiver {
    // ...
}
```

As you can see, this interface file consists of three interfaces: `ISender`, `IRelayer`, and `IReceiver`.

The corresponding `Messaging` contract is also composed of these three parts, implementing and providing the functionalities of these interfaces. While we have already discussed `ISender` and `IReceiver` in previous sections, for more information on `ISender`, please refer to the [Sending](../../build-with-messaging/sending.md) section, and for `IReceiver`, please consult the [Receiver](../../build-with-messaging/receiving.md) section.

Now, let's focus on introducing the `IRelayer` interface.

```solidity
interface IRelayer {
    // ethereum --> anywhere, debug version
    function vMsg(
        uint64 slot,
        bytes calldata message,
        bytes[] calldata accountProof,
        bytes[] calldata storageProof
    ) external;

    // layer 1 --> zkSync
    function vMsgZkSyncL1ToL2(
        bytes calldata messageBytes
    ) external;

    // zkSync --- l2 --> l1
    function vMsgZkSyncL2ToL1(
        // address _zkSyncAddress,
        address _someSender,
        // zkSync block number in which the message was sent
        uint256 _l1BatchNumber,
        // Message index, that can be received via API
        uint256 _proofId,
        // The tx number in block
        uint16 _trxIndex,
        // The message that was sent from l2
        bytes calldata _messageBytes,
        // Merkle proof for the message
        bytes32[] calldata _proof
    ) external;
}
```

The `IRelayer` interface is designed for off-chain Relayers, and we will delve into it in the [next section](relayer.md).&#x20;

This interface comprises three functions:

* `vMsg`, which stands for Verify Message. Its verification process relies on the block header information provided by the On-chain Light Client, as discussed in the previous section through the On-chain Light Client verification method.
* `vMsgZkSyncL1ToL2`: This function is used to verify messages transmitted on zkSync from Layer 1 to Layer 2 and utilizes the message verification methods provided by zkSync.
* `vMsgZkSyncL2ToL1`: This function is used to verify messages transmitted on zkSync from Layer 2 to Layer 1 and also leverages zkSync's message verification methods.
