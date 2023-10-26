# Importing

The repository address for the interfaces and abstract contracts is [TusimaNetwork/zkBridge-messaging-interfaces](https://github.com/TusimaNetwork/zkBridge-messaging-interfaces)

You can import the interface or abstract contract into your project by following these steps.&#x20;

First,

1. Copy the code into your project.
2. Create an `IMessaging.sol`, `Receiver.sol`, or `ReceiverUpgradeable.sol` file in your project at any location, such as `interfaces/`
3. Paste the corresponding code into the file and save it.

Then,

* Import the NPM (Coming soon)
* Import Foundry
  1.  Install dependencies.

      ```sh
      forge install TusimaNetwork/zkBridge-messaging-interfaces
      ```
  2.  Import the interface file.

      ```solidity
      // import IReceiver
      import { IReceiver } from "zkBridge-messaging-interfaces/src/interfaces/IMessaging.sol";

      // import ISender
      import { ISender } from "zkBridge-messaging-interfaces/src/interfaces/IMessaging.sol";

      // import Receiver
      import { Receiver } from "zkBridge-messaging-interfaces/src/interfaces/Receiver.sol";

      // import ReceiverUpgradeable
      import { Receiver } from "zkBridge-messaging-interfaces/src/interfaces/ReceiverUpgradeable.sol";
      ```
