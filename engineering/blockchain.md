---
title: null
description: null
date: null
---

# Best practices in blockchain development

- [N Commandments](#n-commandments)
- [Encryption knowledge](#encryption-knowledge)
  - [Basic concepts](#basic-concepts)
  - [Digital signature extension](#digital-signature)
  - [Merkle tree](#merkle-tree)
- [Blockchain Basics](#blockchain-basics)
  - [Learn the basics of Distributed Ledger Technology]
  - [Ethereum blockchain](#ethereum-blockchain)
  - [Elliptic Curve Cryptography](#elliptic-curve-cryptography)
  - [Proof of Work](#proof-of-work)
  - [Proof of Stake](#proof-of-stake)
  - [Practical Byzantine Fault Tolerance]
- [Development environment setup in README.md]
  - [Hardhat]
  - [Anchor]
- [Blockchain data]
  - [General considerations]
  - [Introduction to Distributed Storage]
  - [Persistence solutions]
    - [IPFS]
    - [SWARM]
- [Learn Solidity]
  - [Solidity Smart contract basic]
  - [Remix IDE]
  - [Smart contract token]
  - [Smart contract NFT]
  - [Deploy and upgrade]
- [Learn Solana Rust]
  - [Rust basic]
  - [Anchor language]
  - [Deploy and upgrade]
  - [Token program]
- [Blockchain RPC]
  - [EVM RPC]
  - [Solana RPC]
  - [IPC RPC]
  - [NEAR RPC]
- [WEB3]
  - [etherJS]
  - [Web3JS]
  - [Provider]
- [EIP]
  - [General]
  - [Most basics]
- [Security]
  - [Private key]
  - [Multisign wallet]
  - [Secrets]
  - [Audit checklist]
- [Checklists](#checklists)
  - [Responsibility checklist](#responsibility-checklist)
  - [Release checklist](#release-checklist)
- [General questions to consider](#general-questions-to-consider)
- [Generally proven useful tools](#generally-proven-useful-tools)

# N Commandments

1. README.md in the root of the repo is the docs
2. Single command run
3. Single command deploy
4. Single command upgrade
5. Repeatable and re-creatable builds
6. Build artifacts bundle a ["Bill of Materials"](#bill-of-materials)
7. Use [UTC as the timezone](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html) all around

# Encryption knowledge

## [Basic concepts](https://www.geeksforgeeks.org/digital-signatures-certificates/)

### Types of Encryption

- Symmetric Encryption
- Asymmetric Encryption
- Public key– Key which is known to everyone. Ex-public key of A is 7, this information is known to everyone.
- Private key– Key which is only known to the person who’s private key it is.

### Digital Signature Multi-signature, Blind signature, Group signature, Ring signature

A digital signature is a mathematical technique used to validate the authenticity and integrity of a message, software, or digital document.

- Key Generation Algorithms: Digital signature is electronic signatures, which assure that the message was sent by a particular sender.
- Signing Algorithms: To create a digital signature, signing algorithms like email programs create a one-way hash of the electronic data which is to be signed.
- Signature Verification Algorithms : Verifier receives Digital Signature along with the data. It then uses Verification algorithm to process on the digital signature and the public key (verification key) and generates some value.

### Merkle Tree

Merkle tree also known as hash tree is a data structure used for data verification and synchronization. It is a tree data structure where each non-leaf node is a hash of it’s child nodes. All the leaf nodes are at the same depth and are as far left as possible. It maintains data integrity and uses hash functions for this purpose.

#### Hash Functions

So before understanding how Merkle trees work, we need to understand how hash functions work. A hash function maps an input to a fixed output and this output is called hash. The output is unique for every input and this enables fingerprinting of data. So, huge amounts of data can be easily identified through their hash.

## Blockchain Basics

- [Learn the basics of Distributed Ledger Technology](https://developer.ibm.com/tutorials/cl-blockchain-basics-intro-bluemix-trs/)
  A distributed ledger is a type of database that is shared, replicated, and synchronized among the members of a decentralized network. The distributed ledger records the transactions, such as the exchange of assets or data, among the participants in the network.

### Ethereum blockchain

- [Elliptic Curve Cryptography](https://medium.com/coinmonks/learn-how-to-code-elliptic-curve-cryptography-a952dfdc20ab)

#### [Proof of Work](https://ethereum.org/en/developers/docs/consensus-mechanisms/pow/#top)

- [Proof of Stake](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/)
- [Practical Byzantine Fault Tolerance](https://blockonomi.com/practical-byzantine-fault-tolerance/)

### Development environment setup

- Single command deployment
- Single command upgrade
- Guide to setup environments.

#### [Hardhat](https://hardhat.org/hardhat-runner/docs/getting-started#quick-start)

- Framework hardhat to develop smart contract ethereum
- Hardhat projects use a local installation of the npm package hardhat to make sure everyone working on the project is using the same version. This is why you need to use npx or npm scripts to run Hardhat.
- Using the Hardhat Toolbox
  You can get our recommended setup by installing [@nomicfoundation/hardhat-toolbox](https://hardhat.org/hardhat-runner/plugins/nomicfoundation-hardhat-toolbox), a single plugin that has everything you need.

When you use it, you'll be able to: - Deploy and interact with your contracts using ethers.js and the hardhat-ethers plugin. - Test your contracts with [Mocha](https://mochajs.org/), [Chai](https://chaijs.com/) and our own Hardhat Chai Matchers plugin. - Interact with Hardhat Network with our Hardhat Network Helpers. - Verify the source code of your contracts with the [hardhat-etherscan](https://hardhat.org/hardhat-runner/plugins/nomiclabs-hardhat-etherscan) plugin. - Get metrics on the gas used by your contracts with the [hardhat-gas-reporter](https://github.com/cgewecke/hardhat-gas-reporter) plugin. - Measure your tests coverage with [solidity-coverage](https://github.com/sc-forks/solidity-coverage) - And, if you are using TypeScript, get type bindings for your contracts with [Typechain](https://github.com/dethcrypto/TypeChain/)

[Multiple Solidity versions] (<https://hardhat.org/hardhat-runner/docs/advanced/multiple-solidity-versions>)

- Hardhat supports projects that use different, incompatible versions of solc. For example, if you have a project where some files use Solidity 0.5 and others use 0.6, you can configure Hardhat to use compiler versions compatible with those files
- So in one code base you can have mutile solidity source code version for optimize.

Common problems

- Out of memory errors when compiling large projects. If you are experiencing this problem, you can use Hardhat's --max-memory argument:

```
npx hardhat --max-memory 4096 compile
```

If you find yourself using this all the time, you can set it with an environment variable in your .bashrc: export HARDHAT_MAX_MEMORY=4096.

#### [Anchor language]

- Using anchor framework to develop smart contracts on Solana network
- Anchor is a framework for Solana's Sealevel runtime providing several convenient developer tools for writing smart contracts.

##### High-level Overview

An Anchor program consists of three parts. The program module, the Accounts structs which are marked with #[derive(Accounts)], and the declare_id macro. The program module is where you write your business logic. The Accounts structs is where you validate accounts. Thedeclare_id macro creates an ID field that stores the address of your program. Anchor uses this hardcoded ID for security checks and it also allows other crates to access your program's address.

##### The Accounts Struct

The Accounts struct is where you define which accounts your instruction expects and which constraints these accounts should adhere to. You do this via two constructs: Types and constraints.
Bạn nên cân nhắc kĩ những gì sẽ được lưu trong account để đảm bảo chi phí và dữ liệu được lưu đầy đủ. Account có giới hạn về kích thước bộ nhớ nên sẽ không thể lưu vô hạn

##### The Program Module

- Your source code should be split down into small modules and linked together appropriately.
- Context is the first parameter passed to the function, through the context can access the list of accounts, program id, remaining accounts.
- Instruction data: As a parameter passed to the function after the context, it will be decoded into types according to the definition. You should use appropriate data types to save processing throughput and cost.
- Errors: You need to define all the errors that the program will return as explicitly as possible. Also for system or network error codes you need to return enough. Anchor Internal Errors are different internal error codes.

## Blockchain data

- General considerations

### Introduction to Distributed Storage

### Persistence solutions

#### IPFS

Pick IPFS when you want store small data such as photo, message, hash, HTML, CSS and JavaScript that implement an application on top of the other decentralized systems.... and want these data exist on a distributed system and not under control of any hosting.

SWARM

Pick SWARM when you want faster loading speed and Swarm has a very strong anti-censorship stance. It incentivizes content agnostic collective storage (block propagation/distribution scheme). Swarm may be better for small chunks / low latency.

[Compare swarm and IPFS](https://github.com/ethersphere/swarm/wiki/IPFS-&-SWARM)

## Solidity - Smart contract

- Solidity Smart contract best practice
  - [General](../engineering/blockchain/general.md) Guiding principles that should be kept in mind during development.
  - [Precautions](../engineering/blockchain/precautions.md) Principles that prevent attacks in general or avoid excessive damage in the worst case scenario.
  - [Document](../engineering/blockchain/documents.md) Guidelines on how to properly document smart contracts and the processes surrounding them.
- Deploy and upgrade
- Be extremely careful about external contract calls, which may execute malicious code and change control flow.
- Understand that your public functions are public, and may be called maliciously and in any order. The private data in smart contracts is also viewable by anyone.
- Keep gas costs and the block gas limit in mind.
- Be aware that timestamps are imprecise on a blockchain, miners can influence the time of execution of a transaction within a margin of several seconds.
- Randomness is non-trivial on blockchain, most approaches to random number generation are gameable on a blockchain.

## Blockchain RPC

- EVM RPC
- Solana RPC
- IPC RPC
  - NEAR RPC

## WEB3

- EtherJS
- Web3JS
- Provider

## EIP Ethereum Improvement Proposals

- [General](https://github.com/ethereum/EIPs/tree/master/EIPS)
  EIP stands for Ethereum Improvement Proposal. An EIP is a design document providing information to the Ethereum community, or describing a new feature for Ethereum or its processes or environment. The EIP should provide a concise technical specification of the feature and a rationale for the feature. The EIP author is responsible for building consensus within the community and documenting dissenting opinions.
- Most basics
  Examples of currently accepted standards include:
  - [EIP-20](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md) Token contract like USDT, USDC ...
  - [EIP-721](https://eips.ethereum.org/EIPS/eip-721)(non-fungible token)
  - [EIP-1155](https://eips.ethereum.org/EIPS/eip-1155) Multi Token Standard
  - [Final] (<https://eips.ethereum.org/erc#final>)

## Security

- Private key
- Multisign wallet
- Secrets
- Audit checklist
