---
description: The features of Tusima zkBridge
---

# Our features

*   **Trustless and Permissionless**

    Tusima zkBridge has implemented an On-chain Light Client within its smart contract based on the Ethereum Light Client Protocol. The On-chain Light Client can provide external access to block header information, enabling fully trustless message verification.

    Tusima zkBridge is committed to developing off-chain nodes that can be run by anyone, including `Operator` and `Relayer`. However, it's important to note that during the current stage, permission controls have been implemented.
*   **Based on Zero-Knowledge Proof**

    By introducing zero-knowledge proofs, complex computational logic from the Ethereum Light Client Protocol can be executed off-chain. This conservation of on-chain resources makes the On-chain Light Client feasible.
*   **One-stop Cross-chain Solution**

    Tusima zkBridge offers a comprehensive suite of solutions, starting from the foundational [On-chain Light Client](how-does-it-work/on-chain-light-client/), extending to [Messaging](build-with-messaging/overview.md) for cross-chain communication, and further to the [Token Bridge](usage-for-token-bridge/connect-to-wallet.md) for asset transfers across blockchains. Smart contract developers can utilize Messaging to create omnichain DApps, while end-users can employ Token Bridge for seamless asset transfers across chains.
*   **Easy to Use**

    Tusima zkBridge is a unified API that enables cross-chain communication through a single set of interfaces. Although there are multiple layer-2 scaling solutions on Ethereum, such as zkSync, Polygon zkEVM, Scroll, and others, each with its own communication API, Tusima zkBridge consolidates these diverse communication methods into a unified API. With Tusima zkBridge, facilitating communication between multiple chains becomes straightforward – all you need to do is implement two interfaces: [ISender](build-with-messaging/interface.md) and [IReceiver](build-with-messaging/interface.md).
