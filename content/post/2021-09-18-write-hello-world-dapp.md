---
categories:
  - Blockchain
tags:
  - helloworld
title: Write Hello World DApp on local Ethereum blockchain
date: 2021-09-18
---

As usual, before starting on a new programming language, we need to set up the environment for development.

Here is the list of required tools:

- NodeJS (https://nodejs.org/en/)
- Visual Studio Code (https://code.visualstudio.com/)
  - Solidity extension (https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)
- Truffle (https://www.trufflesuite.com/truffle)

And now let start writing:

First, we need to install [Truffle](https://www.trufflesuite.com/truffle), this is DApp development framework.

```bash
npm install -g truffle
```

Let's create the first application

## Step 1: Initialize project

Initializing the project by using `truffle init` command.

```bash
╭─~
╰─$ mkdir hello-dapp
╭─~
╰─$ cd hello-dapp
╭─~/hello-dapp
╰─$ truffle init

Starting init...
================

> Copying project files to /Users/mypc/hello-dapp

Init successful, sweet!

Try our scaffold commands to get started:
  $ truffle create contract YourContractName # scaffold a contract
  $ truffle create test YourTestName         # scaffold a test

http://trufflesuite.com/docs

╭─~/hello-dapp
╰─$ ls
contracts         migrations        test              truffle-config.js
```

At this step, Truffle already initialized the project structure for us. From here we can start write our first contract.

## Step 2: Write the contract

```bash
╭─~/hello-dapp
╰─$ cd contracts
╭─~/hello-dapp/contracts
╰─$ touch HelloWorld.sol
╭─~/hello-dapp/contracts
╰─$ ls
HelloWorld.sol Migrations.sol
```

And here is the content of `HelloWorld.sol`

![HelloWorld Solidity Contract](https://res.cloudinary.com/thanh-xyz/image/upload/v1631961898/thanhxyz-blog/first-contract_rgzud8.png)

## Step 3: Write the migration

We need to implement the migration to apply the contract to blockchain:

![Migration for the Contract](https://res.cloudinary.com/thanh-xyz/image/upload/v1631962231/thanhxyz-blog/migration-contract_hc3urt.png)

## Step 4: Migrate and Run the DApp

To run migrate please go back to your console and run the command: `truffle develop` then run `migrate` inside the Truffle Develop CLI

For example here how it looks like on my local:

```bash
╭─~/hello-dapp
╰─$ truffle develop
Truffle Develop started at http://127.0.0.1:9545/

Accounts:
(0) 0x3a69afa92f001aec989cfdb1cf7ebc7a416ca344
(1) 0x4f3d58a1f9cf37610381e08edb788016f4338167
(2) 0xc6579de9f0c0c7e5e832c58296ea23a7eecfd629
(3) 0xbc8ca3b5109d2df8121a108de7777820bb6e57ea
(4) 0xb236ab2839a0c775578da1d4c3217ad21b638e92
(5) 0x504e3f4e872aa69be39ec9eb7a361e5d6d3eb722
(6) 0x2bfcfc2a62acff4a39349c6d6eec35281c419a98
(7) 0xbe9feecfcfb9b80d7d6bb16264c35e9185775da0
(8) 0x7a77c6f8a9d6c9e947cde8e910a4a50d71872d2e
(9) 0x9c1bb3cf757154b2ca2c85d19f1cf3fdf92a424c

Private Keys:
(0) 73d77bb14d999aae350c373ae951c4155fb59ea630c83ec8a39d4e33d43290b0
(1) d2c685eca51a26cf0a50900af77a856ba38481f45657fa79120b50027f437630
(2) d8259bd6ba927cecf53e6c608014ad8be56c1172307b418165efb387e2950e6b
(3) 2d4334dc7573a4ccb47df3c954924e14fb52bca8d3c2c748b67b0cba9031efe0
(4) 15d754de5673c148f82c917fbb7fd80510dd43fe3b8e6e3b3a5c243e1737d15a
(5) 4bdf776ddcaf1444315ace616ac99e7a99acdd33d98be7d2bc2f7482104a66b0
(6) 1a340fc146089095630b4776f1f1bb22d22b20750a478a50943ed8cce8653b0c
(7) 631a7ad00f6f896689c12ca2be3a4af2f01d7c70cf70dc1e00ca91dc055795fd
(8) 331e8f5ec7c94b74d96f072a2a4b665b7314bb697d5b7e764a02ea1d9d7d98d5
(9) 3b95db2825d107460e1073c4d19f9f7b51ecca1eafb1fe4cdd1a0c6a164a45c0

Mnemonic: attitude hawk adult danger solve wait bundle inject share enemy tail submit

⚠️  Important ⚠️  : This mnemonic was created for you by Truffle. It is not secure.
Ensure you do not use it on production blockchains, or else you risk losing funds.

truffle(develop)> migrate

Compiling your contracts...
===========================
> Compiling ./contracts/HelloWorld.sol
> Artifacts written to /Users/thanh/hello-dapp/build/contracts
> Compiled successfully using:
   - solc: 0.5.16+commit.9c3226ce.Emscripten.clang



Starting migrations...
======================
> Network name:    'develop'
> Network id:      5777
> Block gas limit: 6721975 (0x6691b7)


1_initial_migration.js
======================

   Replacing 'Migrations'
   ----------------------
   > transaction hash:    0xb6979cb233008ca866057cb78fc0eec0534cbabc224c09a945c187a96d63dc93
   > Blocks: 0            Seconds: 0
   > contract address:    0x9DEEE546F1F728CA2D7b25CfD2D52828009012fa
   > block number:        1
   > block timestamp:     1631962530
   > account:             0x3a69AFA92F001aec989CFDB1cf7eBc7a416ca344
   > balance:             99.999616114
   > gas used:            191943 (0x2edc7)
   > gas price:           2 gwei
   > value sent:          0 ETH
   > total cost:          0.000383886 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:         0.000383886 ETH


2_create_helloworld_contract.js
===============================

   Deploying 'HelloWorld'
   ----------------------
   > transaction hash:    0x5cd43fb9c54b6a28d41d90b755be60265f8b3d4dcbef58b99a40983a83d4270e
   > Blocks: 0            Seconds: 0
   > contract address:    0x21A7fD5d3eaae32764A4cCA0C0A62Fb1Bad2B937
   > block number:        3
   > block timestamp:     1631962530
   > account:             0x3a69AFA92F001aec989CFDB1cf7eBc7a416ca344
   > balance:             99.999301876
   > gas used:            114781 (0x1c05d)
   > gas price:           2 gwei
   > value sent:          0 ETH
   > total cost:          0.000229562 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:         0.000229562 ETH


Summary
=======
> Total deployments:   2
> Final cost:          0.000613448 ETH


- Blocks: 0            Seconds: 0
- Saving migration to chain.
- Blocks: 0            Seconds: 0
- Saving migration to chain.

truffle(develop)> const instance = await HelloWorld.deployed()
undefined
truffle(develop)> instance.SayHello.call()
'Hello World'
truffle(develop)>
```

To execute the program on blockchain we use some Javascript code to interact with the blockchain.

```bash
truffle(develop)> const instance = await HelloWorld.deployed()
undefined
truffle(develop)> instance.SayHello.call()
'Hello World'
truffle(develop)>
```

![Run migration and the Smart](https://res.cloudinary.com/thanh-xyz/video/upload/v1631962644/thanhxyz-blog/migrate-helloworld-contract_lmxwh5.mp4)

If you can see the Hello World like the screenshot below, then congratulation 🎉! You finished the very first DApp. 📦

![Migration for the Contract](https://res.cloudinary.com/thanh-xyz/image/upload/v1631962966/thanhxyz-blog/Screen_Shot_2021-09-18_at_6.02.37_PM_ozf0yj.png)

Thank you for your reading! And here is the code repository: https://github.com/byhbt/dapp-helloworld
