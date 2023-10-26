# Messaging

Repository: [TusimaNetwork/zkBridge-messaging](https://github.com/TusimaNetwork/zkBridge-messaging)



**Messaging** is the primary module of Tusima zkBridge, providing a set of `Solidity` language APIs. Through these APIs, developers can seamlessly transmit messages from the source chain to the destination chain, thus enabling the creation of omnichain applications.

From the perspective of message verification, the Messaging module currently comprises two cross-chain mechanisms. **One relies on the On-chain Light Client to achieve cross-chain communication, while the other uses its existing message verification mechanism to enable cross-chain communication between the Ethereum mainnet (currently Goerli) and zkRollup solutions like zkSync, Polygon zkEVM, and more.** 

Let's delve into both of these cross-chain mechanisms!
