---
description: A demo of using Tusima zkBridge Messaging.
---

# Build a cross-chain bridge in 3 minutes

This article will guide you through a demo on how to achieve a quick cross-chain token transfer using Tusima zkBridge. This demo will involve three contracts: `TokenMock.sol`, `TokenSender.sol`, and `TokenReceiver.sol`. These contracts will utilize the cross-chain communication capabilities provided by Tusima zkBridge's Messaging component to transfer a token from one blockchain to another in a short amount of time.

{% hint style="info" %}
Notice:

The code presented in this article is intended for illustrating the use of Tusima zkBridge and has not undergone a security audit. Please do not use it in real projects without proper security review.

{% endhint %}



{% hint style="info" %}
Notice:

While this demo employs the Foundry framework, you have the flexibility to use any Solidity-compatible framework, such as Hardhat or Truffle, in your real-world development. To learn more about using the Foundry framework, please refer to this [link](https://book.getfoundry.sh/).

{% endhint %}



Repo: [TusimaNetwork/zkBridge-messaging-demo](https://github.com/TusimaNetwork/zkBridge-messaging-demo)

## Add dependencies

Before we begin, we need to import the interface files provided by Tusima zkBridge:

```sh
$ forge install TusimaNetwork/zkBridge-messaging-interfaces
```

Additionally, you may need to include the auxiliary code from OpenZeppelin.

```shell
# in this demo, we use @v4.8.3
$ forge install OpenZeppelin/openzeppelin-contracts@v4.8.3
```

## Mock Token

In this demo, we will use a simulated Token that includes `mint` and `burn` functions. Here is the interface part of it, `ITokenMock.sol`:

```solidity
pragma solidity ^0.8.16;
import {IERC20} from "lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol";

interface ITokenMock is IERC20 {
    function mint(address _to, uint256 _amount) external;
    function burn(address _owner,uint256 _amount) external;
}
```

 `TokenMock.sol` is an implementation contract that builds upon `ERC20.sol` provided by OpenZeppelin and adds `mint` and `burn` functions:

```solidity
function mint(address _to, uint256 _amount) public virtual override onlyAdmin{
    _mint(_to, _amount);
}

function burn(address _owner, uint256 _amount) public virtual override onlyAdmin{
    _burn(_owner, _amount);
}
```

The `TokenMock.sol` contract will be deployed on both the source and destination chains. On the source chain, the token will be burned, and on the destination chain, it will be minted. Since both functions have `Admin` permissions, please remember to set these permissions after deploying all the contracts.

## TokenSender

Let's now implement the `TokenSender` contract, which will be deployed on the source chain to provide users with the ability to send tokens.

To achieve this, we must send a message from the source chain to the target chain's contract. To do this, we need to call the Tusima zkBridge's `Messaging` contract deployed on the corresponding source chain. The address of this contract can be provided as a parameter in the constructor during deployment.

First, let's import the `ISender` interface:

```solidity
import {ISender} from "lib/zkBridge-messaging-interfaces/src/interfaces/IMessaging.sol";
```

The constructor will include two parameters: the address of the `Messaging` contract and the address of the token contract.

```solidity
ISender public messaging;
address public tokenMock;

constructor(address _tusimaMessaging, address _tokenMock) {
    messaging = ISender(_tusimaMessaging);
    tokenMock = _tokenMock;
}
```

Next, we'll write the `sendToken` function, which is defined with four parameters:

- `_destinationChainId`: The ID of the destination chain.
- `_msgReceiver`: The receiving contract on the destination chain.
- `_tokenAmount`: The quantity of tokens to be sent.
- `_receiver`: The address of the token receiver.

There are three steps using this function:

1. Burn the caller's Token

   ```solidity
   ITokenMock(tokenMock).burn(msg.sender, _tokenAmount);
   ```

2. Package the message

   ```solidity
   bytes memory message = abi.encodePacked(_tokenAmount, _receiver);
   ```

3. Send the message

   ```solidity
   messaging.send(_destinationChainId, _msgReceiver, message);
   ```

Here is the full function:

```solidity
function sendToken(
    uint32 _destinationChainId,
    address _msgReceiver,
    uint256 _tokenAmount,
    address _receiver
) external {
    ITokenMock(tokenMock).burn(msg.sender, _tokenAmount);
    bytes memory message = abi.encodePacked(_tokenAmount, _receiver);
    messaging.send(_destinationChainId, _msgReceiver, message);
}
```

## TokenReceiver

The `TokenReceiver` will be deployed on the destination chain to receive messages from the source chain, mint tokens, and send them to the recipient's address.

To realize the function of receiving messages, we just need to implement the abstract contract `Receiver` provided by Tusima zkBridge.

Let's start by introducing `Receiver.sol`:

```solidity
import {Receiver} from "lib/zkBridge-messaging-interfaces/src/interfaces/Receiver.sol";
```

Creating a `TokenReceiver` contract and inherits the abstract contract `Receiver`:

```solidity
contract TokenReceiver is Receiver {}
```

The constructor also takes two parameters, `_tusimaMessaging` and `_tokenMock`:

```solidity
address public tokenMock;

constructor(
    address _tusimaMessaging,
    address _tokenMock
) Receiver(_tusimaMessaging) {
    tokenMock = _tokenMock;
}
```

The abstract contract `Receiver` contains an abstract function `handleMsgImpl` which we need to implement and which will be triggered when a message is received.

There are two things we need to do when implementing the `handleMsgImpl` function. 

1. Parsing the message,
2. Mint Tokens.

The complete function is as follows:

```solidity
function handleMsgImpl(
    uint32 _sourceChainId,
    address _sourceAddress,
    bytes memory _message
) internal override {
    Info memory info = decodeInfo(_message);

    ITokenMock(tokenMock).mint(info.receiver, info.tokenAmount);

    emit TokenMessageReceived(_sourceChainId, _sourceAddress, info);
}

function decodeInfo(
    bytes memory _message
) public pure returns (Info memory info) {
    uint256 tokenAmount; // 256 / 8 = 32
    address receiver; // 20 bytes
    assembly {
        tokenAmount := mload(add(_message, 32))
        receiver := mload(add(_message, 52))
    }
    info.tokenAmount = tokenAmount;
    info.receiver = receiver;
}
```

At this point, all contracts are complete. You can deploy the contracts to any of our supported chains, but please don't forget to assign the `Admin` rights of the `TokenMock` contract to the `TokenSender` and `TokenReceiver` contracts after deployment.

OK, now let's go on a cross-chain journey together.