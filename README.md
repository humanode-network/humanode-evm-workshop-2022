# Intro

This workshop is primarily for developers who understand the basics of evm (Ethereum virtual
machine) smart contracts. It covers environment setup, and basic smart contract development and deployment to the Humanode
network using Truffle or Remix.

# Requirements

- Web browser (Firefox or Chrome)
- Linux or Mac
- Familiarity with the terminal
- npm
- MetaMask
- wget or curl

# Setup

First we need to setup our dev node. This is useful for testing changes in a sandbox environment,
where we don't have to worry about destructive action.

To begin, let's create a directory for our node:

    mkdir humanode-peer
    cd humanode-peer

Now to download the humanode-peer bindary.

The URL of the binary is different depending on OS and architecture. Use the following manifest file
to determine the correct location for your system: https://chainspec.testnet3.stages.humanode.io/latest/manifest.json

The URL for your system is calculated using the base url + suburl:
https://chainspec.testnet3.stages.humanode.io{subUrl} where {subUrl} is the value found in the
manifest file above.

Download binary (Linux x86_64):

    wget https://chainspec.testnet3.stages.humanode.io/latest/binaries/Linux-x86_64/humanode-peer

Download binary (Darwin x86_64):

    wget https://chainspec.testnet3.stages.humanode.io/latest/binaries/Darwin-x86_64/humanode-peer

Now we have to change the file permisions to make it executable:

    chmod +x humanode-peer

Finally run the executable:

    ./humanode-peer --tmp --dev

# Using Truffle

[Truffle](https://trufflesuite.com/) is a suite of tools for smart contract development.

Humanode provides a [truffle box](https://github.com/humanode-network/humanode-truffle-box) that can
be used to develop and deploy smart contracts to the Humanode network. It can be used as a template,
or unboxed using the `truffle unbox` command. To use as a template, click the "use template" button
in github and follow the instructions in the readme.

Instructions for using `truffle unbox` are below.

First we create a new directory for our truffle project:

    mkdir my-contracts
    cd my-contracts

Make sure you have truffle installed:

    npm install -g truffle

Next unbox the Humanode truffle box (this can take a while):

    truffle unbox humanode-network/humanode-truffle-box

Write your contracts (example contract shown below).

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.7.0;

// Import OpenZeppelin Contract
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

// This ERC-20 contract mints the specified amount of tokens to the contract creator.
contract MyToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("MyToken", "MYTOK") {
        _mint(msg.sender, initialSupply);
    }
}
```

Test contracts on the Humanode network:

    truffle test --network dev

Deploy contracts to the Humanode network:

    truffle deploy --network dev

# Using Remix

Remix is a web IDE for quick and easy smart contract development and deployment. To use Remix, we
first need to conntect our local node to MetaMask.

Add our network to MetaMask through `Settings -> Networks -> Add a network`, and enter the following
details:

**Network name**: Humanode DEV

**RPC URL**: http://127.0.0.1:9933

**Chain ID**: 5234

**Currency Symbol**: HMND


To deploy and run contracts you need an account with gas. A pre-funded dev account is provided.
Import the following address into MetaMask: `0x99b3c12287537e38c90a9219d4cb074a89a16e9cdb20bf85728ebd97c343e342`.

Go to https://remix.ethereum.org/

Compile contract, and deploy with "Injected Web3".

# Extra

- Polkadotjs: use the polkadot explorer to debug chain events and transactions: https://polkadot.js.org/apps/#/explorer

- Desktop app: we've downloaded the binary directly, but everything you see here is also possible using
    the desktop app. Please review the humanode docs for more information: https://desktop-app-docs.humanode.io/

- Open source: we plan on taking humanode open-source, however this will be after mainnet launch.
    We don't want people to use it before it's ready.

- Pallet EVM: Humanode is built using Substrate, which is a framework for making blockchains.
    It lets you compose your blockchain with building blocks called "pallets". One of the pallets we
    use is called pallet_evm, which implements the Ethereum virutal machine. This means any smart
    contract that runs on Ethereum can also run on Humanode.
