---
sidebar_label: Using Foundry
sidebar_position: 500
title: Rootstock Foundry Starter kit
description: 'Whether you are a seasoned developer or just starting your journey into smart contract development, the foundry starter kit provides a solid foundation for building decentralized applications (dApps) on the Rootstock network.'
tags: [rsk, rootstock, tutorials, developers, foundry, quick starts, dApps, smart contracts]
---

Whether you’re a seasoned developer or just starting your journey into smart contract development, the foundry starter kit provides a solid foundation for building decentralized applications (dApps) on the Rootstock network.

Rootstock is fully EVM (Ethereum Virtual Machine) compatible. It brings the power of smart contracts to Bitcoin, allowing developers to leverage Bitcoin’s security while benefiting from Ethereum’s ecosystem.

## Prerequisites

Before starting the dApp, make sure to have the following prerequisites:

1. **Familiarity with Smart Contracts:**
   - If you’re new to smart contracts, consider learning the basics. Understanding how smart contracts work will enhance your experience with Rootstock development.

2. **Foundry installation using [Foundryup](https://book.getfoundry.sh/getting-started/installation#using-foundryup):**
- This installing information is taken from the official [Foundry documentation](https://book.getfoundry.sh/getting-started/installation#using-foundryup), in case you need to go in detail.
- Foundryup is the official installer for the Foundry toolchain. You can learn more about it [here](https://github.com/foundry-rs/foundry/blob/master/foundryup/README.md).
- If you encounter any issues during installation, refer to the Foundryup [FAQ](https://book.getfoundry.sh/faq.html) for assistance.
- Precompiled binaries can be downloaded from the Foundry [GitHub releases page](https://github.com/foundry-rs/foundry/releases). For easier management, we recommend using Foundryup.

To install Foundry in your system, run the following command:
```bash
curl -L https://foundry.paradigm.xyz | bash
```
This will install Foundryup. Simply follow the on-screen instructions, and the ```foundryup``` command will become available in your CLI.

Running ```foundryup``` will automatically install the latest (nightly) versions of the ```precompiled binaries```: ```forge```, ```cast```, ```anvil```, and ```chisel```. For additional options, such as installing a specific version or commit, run ```foundryup --help```.

**_NOTE: If you’re using Windows, you’ll need to install and use [Git BASH](https://gitforwindows.org/) or [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) as your terminal, since Foundryup currently doesn’t support Powershell or Command Prompt (Cmd)._** 

3. **Basic Knowledge of Foundry:**
- Familiarity with Foundry's core concepts and functionalities is recommended. If you're new to Foundry, refer to the [Rootstock Foundry Guide](/developers/smart-contracts/foundry/).

:::tip[Rootstock Blockchain Developer Course]

Learn how to write, test, secure, deploy and verify smart contracts on the Rootstock blockchain network. Enroll for the [Rootstock Blockchain Developer Course](/resources/courses/).
:::

## Setting Up the Sample dApp

### Clone the Repository

Open your terminal or command prompt and run the following command to clone the repository from GitHub:

```bash
git clone https://github.com/rsksmart/rootstock-foundry-starterkit.git
```

### Install Dependencies

Navigate to the cloned repository folder:

```bash
cd rootstock-foundry-starterkit
```

Install all required dependencies using forge:

```bash
forge install OpenZeppelin/openzeppelin-contracts
```

### Obtain Rootstock Testnet and Mainnet RPC URLs

This section will walk you through adding Rootstock Testnet and Mainnet RPC URLs to your development environment. These URLs are essential for connecting your application to the Rootstock network and interacting with smart contracts.

There are two ways to obtain RPC URLs:

#### Using Public RPC URLs

- Visit the [MetaMask Integration on the Rootstock Dev Portal](/dev-tools/wallets/metamask/). This guide provides instructions on setting up MetaMask for Rootstock. While following these steps, pay close attention to the sections on adding custom networks. You'll find the RPC URLs for Rootstock Testnet and Mainnet listed.

#### Using RPC API
- Create an account at the [Rootstock RPC API](https://rpc.rootstock.io/). Once logged in, navigate to your dashboard and copy the API Key. 


### Adding environment variables to your project

After obtaining the RPC URLs, create a file named `.env` in your project's root directory (important: this file should not be committed to version control). Add the necessary environment variables to the `.env` file:
```
PRIVATE_KEY: Your private key (e.g., from your Metamask account details).
```
**_NOTE: Make sure the private key you paste, starts with 0x, if does not contain it, please put 0x before your private key. (0x123...)_** 

## Running tests to an ERC20 Token Contract
This section runs tests on an ERC20 token contract (fungible token), this is done according to the script located at ```test/Erc20Token.t.sol```. It does test deployment, minting, and transfer of tokens.

For this, run the next forge command:

```bash
forge test
```

It should return an output similar to the following:

```bash
Compiler run successful!

Ran 2 tests for test/Erc20Token.t.sol:ERC20TokenTest
[PASS] testInitialSupply() (gas: 9849)
[PASS] testTransfer() (gas: 43809)
Suite result: ok. 2 passed; 0 failed; 0 skipped; finished in 8.73ms (1.51ms CPU time)

Ran 1 test suite in 143.90ms (8.73ms CPU time): 2 tests passed, 0 failed, 0 skipped (2 total tests)
```
**_NOTE: If you need additional tests, or want to go deep on this step, visit the [Foundry Tests Documentation](https://book.getfoundry.sh/forge/tests)._** 

## Deploying an ERC20 Token Contract
This section deploys an ERC20 token contract (fungible token) on the Rootstock network. This contract is located at ```src/Erc20Token.sol``` file, it uses the script located at ```script/Deploy.s.sol``` for this operation.

Run the following command, replacing `https://public-node.testnet.rsk.co` with either `rskTestnet` or `rskMainnet` rpc url if you have, depending on your desired deployment environment:

```bash
forge script script/Deploy.s.sol --rpc-url https://public-node.testnet.rsk.co --broadcast --legacy
```
:::info[Info]

- [EIP-1559](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md) is not supported or not activated on the Rootstock RPC url
- The `--legacy` flag is passed to use legacy transactions instead of `EIP-1559`.
- You can remove the `--broadcast` flag if you wan to simulate the transaction without broadcasting it.
:::

> If you get an error like `Transaction dropped from the mempool: <tx-id>` or the ```transaction not completed```, check the tx-id in the explorer. The tx may have went successful but the error is still in the logs. Here are the [mainnet](https://explorer.rootstock.io/) and [testnet](https://explorer.testnet.rootstock.io/) explorers.

> Also you can see the transaction registry locally, by checking the folder ```broadcast/Deploy.s.sol/``` and opening the file called ```run-latest.json```, if you check the fields, there is one called ```contractAddress``` which contains the new address deployed for our ERC20 smart contract.

The result should look like this:
```bash
Sending transactions [0 - 0].
⠁ [00:00:00] [###############################################################################################################################################] 1/1 txes (0.0s)##
Waiting for receipts.
⠉ [00:00:25] [###########################################################################################################################################] 1/1 receipts (0.0s)
##### 31
✅  [Success]Hash: 0x48ea2b06b39cd436a2d7564e20ea5bb598ddc2769e6b18c855170f0e9e4d5687
Contract Address: 0x499e802a6825d30482582d9b9dd669ba82ba8ba4
Block: 5071408
Gas Used: 106719

==========================

ONCHAIN EXECUTION COMPLETE & SUCCESSFUL.
Total Paid: 0. ETH (106719 gas * avg 0 gwei)
```

## Interacting with the Contract
If the contract is already deployed, then you can interact with it using ```cast``` this command allows you to interact with the contract, in this case, read the balance of an account.

### Reading the Balance of an Account
In your terminal, run the following command, replacing the placeholders with actual values:

```bash
cast call <contract_address> "balanceOf(address)(uint256)" <wallet_address> --rpc-url <rpc_url>
```
The result should look like this:
```bash
1000000000000000000000 [1e21]
```

## Final Comments

Explore and be curious about the folders and files of this starter kit. You'll learn how to import `.env` variables for deploying smart contracts, test smart contracts with solidity. Feel free to customize this starter kit to suit your project’s needs. Happy coding!