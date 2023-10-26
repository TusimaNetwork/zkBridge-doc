# Sending

After created the Receiver contract, we create a Sender contract, like `MsgSender.sol`:

```solidity
import { ISender } from "zkBridge-messaging-interfaces/src/interfaces/IMessaging.sol";

contract MsgSender {
    
    ISender public tusimaMessaging;

    constructor(address _tusimaMessaging) {
        tusimaMessaging = ISender(_tusimaMessaging);
    }

    function sendMsg(
        uint32 _targetChainId, 
        address _targetAddr, 
        bytes memory _message
    ) external
    {
        string memory data = "Hello world";
        tusimaMessaging.send(_targetChainId, _targetAddr, bytes(data));
    }
}
```

Like `MsgReceiver.sol`, we need to pass in the `_tusimaMessaging` parameter (please refer to [Contracts](../contracts.md)) when deploying .

After deployment is completed, we can send messages by calling the `sendMsg` function, which has the following three parameters:

1. `_targetChainId`, the chain id of the destination chain, for example, the BSC test net's chain id is 97, about more chain ids you can go to [Helpful information about supporting chain](../../helpful-information-about-supporting-chain.md).
2. `_targetAddr`, the Receiver contract address that deployed on the destination chain, that is the address of  `MsgReceiver.sol`.
3. `_message`, the message you want to send, for example "Hello world".
