# Receiving

Before sending a message, you should first create a Receiver contract for message reception, for example, `MsgReceiver.sol`.

Because we have the abstract contract `Receiver.sol`, all you need to do is inherit from the `Receiver` contract and implement the `handleMsg` function as defined.

1. As mentioned in the [previous section](importing.md), you should import the interface.
2.  Here are the steps to create the `MsgReceiver.sol` contract:

    ```solidity
    import { Receiver } from "zkBridge-messaging-interfaces/src/interfaces/Receiver.sol";

    contract MsgReceiver is Receiver {

        string public message;

        constructor(address _tusimaMessaging) Receiver(_tusimaMessaging) {}

        function handleMsgImpl(
            uint32 _sourceChainId, 
            address _sourceAddress, 
            bytes memory _message
        )internal override
        {
            message = string(_message);
            // you can do anything here
        }
    }
    ```
3. During deployment, you need to provide the `_tusimaMessaging` parameter, which represents the address of the `Messaging` contract on the destination chain. For specific details, please refer to [Contracts](../contracts.md).
