---
categories:
  - Blockchain
tags:
  - helloworld
title: Write Hello World DApp on local Ethereum blockchain
date: 2021-09-18
---

As usual, before starting on new programming language, we need to set up the environment for development.

Here is the list of required tools:

- NodeJS (https://nodejs.org/en/)
- Visual Studio Code (https://code.visualstudio.com/)
  - Solidity extension (https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)
- Truffle (https://www.trufflesuite.com/truffle)

And now let start writing:

First we need to install [Truffle](https://www.trufflesuite.com/truffle), this is DApp development framework.

```bash
npm install -g truffle
```

Then let start to create the first application

## Step 1: Initialize project

```bash
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

![Run migration and the Smart ](https://res.cloudinary.com/thanh-xyz/video/upload/v1631962644/thanhxyz-blog/migrate-helloworld-contract_lmxwh5.mp4)

If you can see the Hello World like the screenshot below, then congratulation! You finished the very first DApp.

![Migration for the Contract](https://res.cloudinary.com/thanh-xyz/image/upload/v1631962966/thanhxyz-blog/Screen_Shot_2021-09-18_at_6.02.37_PM_ozf0yj.png)
