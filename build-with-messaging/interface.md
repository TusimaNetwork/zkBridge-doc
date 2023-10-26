# Interface

We offer developers an interface file named `IMessaging.sol` as follows:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

enum MessageStatus {
    NOT_EXECUTED,
    EXECUTION_FAILED,
    EXECUTION_SUCCEEDED
}

struct Message {
    uint8 version;
    uint64 nonce;
    uint32 sourceChainId;
    address sourceAddress;
    uint32 destinationChainId;
    bytes32 destinationAddress;
    bytes data;
}

interface ISender {
    function send(
        uint32 targetChainId,
        address targetAddr,
        bytes calldata message
    ) external returns (bytes32);
}

interface IReceiver {
    function handleMsg(
        uint32 sourceChainId,
        address sourceAddr,
        bytes memory message
    ) external returns (bytes4);
}

```

This file includes two interfaces, `ISender` and `IReceiver`, corresponding to Sending and Receiving functionalities, respectively.

You can implement the message receiving function yourself using the `IReceiver` interface. However, you must ensure the following:

* Verify that `msg.sender` originates from Tusima's `Messaging` contract.
* Return the function selector at the end of the function, i.e., `return IReceiver.handleMsg.selector`.

We also provide a simpler usage method, which you can find in the [next section](abstract-contract.md).
