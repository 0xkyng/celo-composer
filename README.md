# How to use Celo Composer to build a simple decentralized application (DApp) on the Celo blockchain

## Table of Content
- [How to use Celo Composer to build a simple decentralized application (DApp) on the Celo blockchain](#how-to-use-celo-composer-to-build-a-simple-decentralized-application-dapp-on-the-celo-blockchain)
  - [Table of Content](#table-of-content)
  - [Introdcution](#introdcution)
  - [Prerequisites:](#prerequisites)
  - [What Is Celo?](#what-is-celo)
  - [What Is the Celo Composer?](#what-is-the-celo-composer)
  - [How to Create, Test and Deploy Smart Contracts With Celo Composer](#how-to-create-test-and-deploy-smart-contracts-with-celo-composer)
    - [Getting Started with Celo Composer](#getting-started-with-celo-composer)
    - [How to Create a New Project](#how-to-create-a-new-project)
    - [Creating a Smart Contract](#creating-a-smart-contract)
    - [Testing the Smart Contract with Hardhat](#testing-the-smart-contract-with-hardhat)
    - [Deploying the Smart Contract to the Celo Network](#deploying-the-smart-contract-to-the-celo-network)
  - [Build a simple frontend](#build-a-simple-frontend)
  - [Conclusion](#conclusion)

## Introdcution

In this article, you will learn about the following concepts:
- The Celo blockchain.
- The Celo Composer.
- How to use Celo Composer to build a simple decentralized application (DApp) on the Celo blockchain.

## Prerequisites:

1. Familiarity with Solidity programming language.
2. Basic knowledge of smart contracts and DApp development.
3. Familiarity with Celo blockchain and its ecosystem.
4. Familiarity with react.

Before we begin, make sure you have the following installed on your computer:

- [Node.js](https://nodejs.org/) and NPM
- [Hardhat](https://hardhat.org/)
- [Celo Wallet](https://celowallet.app/setup)

So now, let's delve in.

## What Is Celo?
**[Celo](https://docs.celo.org/learn/celo-overview)** is a mobile-first blockchain that makes decentralized financial (DeFi) tools and services accessible to anyone with a mobile phone. It aims to break down barriers by bringing the powerful benefits of DeFi to the users of the six billion smartphones in circulation today.

It is designed to provide a stablecoin (cUSD) that is pegged to the US dollar, as well as a suite of decentralized applications (dApps) that can be used to access financial services such as remittances, loans, and micropayments.

## What Is the Celo Composer?
**[Celo Composer](https://github.com/celo-org/celo-composer)** is a tool built by the Celo team that allows you to quickly build, deploy, and iterate on decentralized applications using Celo. It provides a number of frameworks, examples, and Celo-specific functionality to help you get started with your next dApp.

## How to Create, Test and Deploy Smart Contracts With Celo Composer

### Getting Started with Celo Composer

You'll need to install the Celo Composer CLI, which is a command-line interface that interacts with the Celo network in other to get started. This is because Celo Composer is an open-source tool that is available on GitHub.

But firstly, ensure that you have **[NodeJS](https://nodejs.org/en/download)** and **[NPM](https://www.npmjs.com/)** installed on your computer. Next, open a terminal and run the following command to install the Composer CLI:

```bash
npm install -g @celo/contractkit
```

You can run the command below to verify that the CLI is installed correctly, once the installation is complete:

```bash
celocli
```

This command should output the list of available commands for the CLI.

Next, you'll need to install Hardhat. You can install Hardhat using the following command:

```bash
npm install --save-dev hardhat
```

### How to Create a New Project 

To create a new project, you can use the following command:

```bash
celocli init
```

The above command will create a new project with the basic files and directories required to create and deploy smart contracts.

The project structure should look like this:

```
contracts
migrations
test
hardhat-config.js
```
The contracts directory contains the smart contract files that you'll create using the Celo Composer. The migrations directory will contain migration scripts that are used to deploy the smart contracts to the Celo network. The test directory will contain the tests for the smart contracts.

Next, you'll need to configure Hardhat. To do this, create a new file called `hardhat.config.js` in the root directory of your project and paste the following code:

```js
require("@nomiclabs/hardhat-waffle");
require("@nomiclabs/hardhat-ethers");

module.exports = {
    solidity: "0.8.0",
    networks: {
        hardhat: {},
    },
};
```
The above configuration file specifies the Solidity version and sets up the Hardhat network.

### Creating a Smart Contract

To create a new smart contract, you can use the following command:

```bash
celocli create:contract
```

To configure your project, adhere to the directions. A project name, the Celo network you want to use (such as Alfajores, Baklava, or Mainnet), and other pertinent data must be included.

Once initialization is complete, a fresh Celo Composer project will be created for you with a basic folder structure and a contracts directory where your smart contracts can be written.

For the sake of this article, the smart contract file will contain the basic structure of a Celo smart contract and a simple dApp.

```solidity

pragma solidity ^0.8.0;

contract EgoToken {
    string public name;
    string public symbol;
    uint256 public totalSupply;
    mapping(address => uint256) public balances;

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor(string memory _name, string memory _symbol, uint256 _totalSupply) {
        name = _name;
        symbol = _symbol;
        totalSupply = _totalSupply;
        balances[msg.sender] = _totalSupply;
    }

    function transfer(address to, uint256 value) external returns (bool) {
        require(balances[msg.sender] >= value, "Insufficient balance");
        balances[msg.sender] -= value;
        balances[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }
}
```
Let's go through the code line by line;


```contract EgoToken```: This declares a contract named EgoToken, which represents the smart contract that will be deployed on the blockchain.

```string public name```: This declares a public string variable named `name` that will store the name of the token.

```string public symbol```: This declares a public string variable named `symbol` that will store the symbol of the token.

```uint256 public totalSupply```: This declares a public unsigned integer variable named `totalSupply` that will store the total supply of the token.

```mapping(address => uint256) public balances```: This declares a public mapping named `balances` that will map addresses with their corresponding token balances.

```event Transfer(address indexed from, address indexed to, uint256 value)```: This declares an event named `Transfer` that will be emitted when tokens are transferred between addresses. The event includes the from address, the to address, and the value (amount) of tokens transferred.

`constructor(string memory _name, string memory _symbol, uint256 _totalSupply)`: This is the constructor function that initializes the smart contract when it is deployed. It takes three parameters: `_name` (the name of the token), `_symbol` (the symbol of the token), and `_totalSupply` (the total supply of the token).

`name = _name`: This assigns the value of `_name` to the name variable.

`symbol = _symbol`: This assigns the value of `_symbol` to the symbol variable.

`totalSupply = _totalSupply`: This assigns the value of `_totalSupply` to the `totalSupply` variable.

`balances[msg.sender] = _totalSupply`: This initializes the balance of the `msg.sender` (the address that deploys the contract) with the total supply of tokens.

`function transfer(address to, uint256 value) external returns (bool)`: This is a function named `transfer` that allows users to transfer tokens to another address. It takes two parameters: to (the address to which tokens will be transferred) and value (the amount of tokens to be transferred).

`require(balances[msg.sender] >= value, "Insufficient balance")`: This checks if the sender (msg.sender) has enough tokens to transfer. If the balance is not sufficient, it will revert the transaction with an error message "Insufficient balance".

`balances[msg.sender] -= value`: This subtracts the value (amount) of tokens from the sender's balance after a successful transfer.

`balances[to] += value`: This adds the value (amount) of tokens to the receiver's (to) balance.

`emit Transfer(msg.sender, to, value)`: This emits the Transfer event, indicating that tokens have been transferred from the sender (msg.sender) to the receiver (to) with the amount of value tokens.

`return true`: This returns a boolean value true indicating a successful transfer.

`function balanceOf(address account) external view returns (uint256)`: This is a function named `balanceOf` that allows users to retrieve the balance of tokens for a specific address.

### Testing the Smart Contract with Hardhat

Before deploying the smart contract to the Celo network, you'll need to test it to ensure that it works as expected. But first, you need to install some additional dependencies to enable Hardhat to interact with the Celo network. You'll need to install `@celo/contractkit` and `@nomiclabs/hardhat-etherscan` as follows:

`npm install --save-dev @celo/contractkit @nomiclabs/hardhat-etherscan`

Once these dependencies have been successfully installed, you can create a new test file in the `test` directory of our project, called `EgoToken.test.js`.


Import dependencies: In your test file, import the necessary dependencies, including Hardhat, ethers.js (the Ethereum and Celo JavaScript library). For example:

```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");
```

Then, write test cases. Define your test cases using the `describe` and `it` functions provided by Mocha, the testing framework that comes with Hardhat. For example:

```javascript
describe("EgoToken Test Suit", function () {
  let egoToken;
  let owner;
  let isaac;

  beforeEach(async function () {
    // Deploy the EgoToken contract
    [owner, isaac] = await ethers.getSigners();
    egoToken = await ethers.getContractFactory("EgoToken").deploy();
    await egoToken.deployed();
  });

  it("should have correct initial values", async function () {
    // check initial token supply and owner balance
    expect(await egoToken.totalSupply()).to.equal(ethers.BigNumber.from("1000000000000000000000000"));
    expect(await egoToken.balanceOf(owner.address)).to.equal(ethers.BigNumber.from("1000000000000000000000000"));
  });

  it("should transfer tokens correctly", async function () {
    // Transfer tokens from owner to isaac
    await egoToken.connect(owner).transfer(isaac.address, ethers.utils.parseEther("100"));

    // check balances after transfer
    expect(await egoToken.balanceOf(owner.address)).to.equal(ethers.BigNumber.from("999999999999999999999900"));
    expect(await egoToken.balanceOf(isaac.address)).to.equal(ethers.BigNumber.from("100"));
  });

});
```

Let's now go through this code step by step:
1. `beforeEach`: This is a special function that is executed before each test case. In this case, it deploys the EgoToken contract using the contract factory, gets the `owner` and `isaac` signers, and assigns the deployed contract instance to the `egoToken` variable.

2. `it("should have correct initial values", async function () {...})`: This is the first test case. It checks if the initial values of the `totalSupply` and the `balanceOf` the owner's address match the expected values.

3. `it("should transfer tokens correctly", async function () {...})`: This is the second test case. It tests the transfer function of the `EgoToken `contract by transferring tokens from the `owner `to `isaac` address and then checks if the balances of `owner` and `alice` are updated correctly after the transfer.

You will then use the following command to run the test:

```bash
npx hardhat test
```

This will run the `EgoToken.test.js` test file and output the results.

### Deploying the Smart Contract to the Celo Network

Now that you've tested the smart contract and confirmed that it works as expected, you can deploy it to the Celo network using Celo Composer.

But first, you'll need to make sure that you have the Celo network configuration set up in your `hardhat.config.js` file. You can do this by adding the following code:

```javascript
require("@nomiclabs/hardhat-waffle");
require("@nomiclabs/hardhat-etherscan");
require("@celo/hardhat-plugin");

const mnemonic = process.env.MNEMONIC;

module.exports = {
  solidity: {
    compilers: [
      {
        version: "0.8.4",
      },
    ],
  },
  networks: {
    hardhat: {},
    alfajores: {
      url: "https://alfajores-forno.celo-testnet.org",
      accounts: { mnemonic: mnemonic },
      gasPrice: 1000000000,
    },
    mainnet: {
      url: "https://forno.celo.org",
      accounts: { mnemonic: mnemonic },
      gasPrice: 1000000000,
    },
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY,
  },
  celo: {
    chainId: 44787,
    from: "0x<YOUR_CELO_ADDRESS>",
    gas: 10000000,
    gasPrice: 1000000000,
  },
};
```

This sets up the `alfajores` and `mainnet` networks, which we can use to deploy our contract to the Celo network.

Once you have a Celo account set up, you can use Celo Composer to deploy your `EgoToken` contract to the Celo network.

Compile the contract using the following command:

```bash
celocli compile
```

Deploy the contract to the Celo network using the following command:

```bash
celo-composer deploy --contract MyToken --args "EgoToken,EGT,1000000"

```

This command deploys the `EgoToken` smart contract with the provided constructor arguments (in this case, "EgoToken" for the name, "EGT" for the symbol, and 1000000 for the total supply). The --contract flag specifies the name of the smart contract you want to deploy.

After running the deployment command, you'll receive a transaction hash. You can use a Celo blockchain explorer (e.g., https://alfajores-blockscout.celo-testnet.org/ for Alfajores network) to confirm the deployment status and verify that your smart contract has been successfully deployed.

Congratulations! You have successfully deployed your `EgoToken` contract to the Celo network using the Celo Composer. You can now interact with your contract using the **celocli** command line tool or any other Celo-compatible wallet or application.

## Build a simple frontend

Let's build a simple frontend for the `EgoToken` contract using React and ethers.js.

1. Install dependencies:

```bash
npm install ethers react react-dom
```

2. Create a React component for the frontend:

```jsx
import React, { useState, useEffect } from "react";
import { ethers } from "ethers";
import EgoTokenABI from "./EgoToken.json"; // Import the ABI of your EgoToken contract

const EgoTokenApp = () => {
  const [contract, setContract] = useState(null);
  const [ownerBalance, setOwnerBalance] = useState(0);

  useEffect(() => {
    // Connect to the Ethereum provider
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    // Create an instance of the EgoToken contract
    const egoTokenContract = new ethers.Contract(
      "CONTRACT_ADDRESS", // Replace with the address of your deployed EgoToken contract
      EgoTokenABI, // Pass the ABI of your EgoToken contract
      provider.getSigner()
    );
    setContract(egoTokenContract);
  }, []);

  useEffect(() => {
    // Fetch the owner's balance
    const fetchOwnerBalance = async () => {
      if (contract) {
        const balance = await contract.balanceOf(contract.signer.getAddress());
        setOwnerBalance(balance.toNumber());
      }
    };
    fetchOwnerBalance();
  }, [contract]);

  const handleTransferTokens = async () => {
    if (contract) {
      const tx = await contract.transfer("RECEIVER_ADDRESS", ethers.utils.parseEther("100")); // Replace with the address of the receiver
      await tx.wait();
      console.log("Tokens transferred successfully!");
      // Update the owner's balance after the transfer
      const balance = await contract.balanceOf(contract.signer.getAddress());
      setOwnerBalance(balance.toNumber());
    }
  };

  return (
    <div>
      <h1>EgoToken App</h1>
      <h2>Owner's Balance: {ownerBalance} Tokens</h2>
      <button onClick={handleTransferTokens}>Transfer Tokens</button>
    </div>
  );
};

export default EgoTokenApp;
```

Note: Please replace the `CONTRACT_ADDRESS` with the actual address of your deployed `EgoToken` contract, and `RECEIVER_ADDRESS` with the address of the receiver for the `handleTransferTokens` function.

3. Use the EgoTokenApp component in your main React app:

```jsx
import React from "react";
import EgoTokenApp from "./EgoTokenApp";

const App = () => {
  return (
    <div>
      <EgoTokenApp />
    </div>
  );
};

export default App;
```

Next, let's connect to the Celo blockchain using `celo-sdk.js`: `celo-sdk.js` is a JavaScript library that provides a convenient way to interact with the Celo blockchain. To use it in your frontend DApp, you can install it as a dependency using npm:

```bash
npm install celo-sdk
```

Once installed, you can import `celo-sdk.js` in your code and use it to connect to the Celo blockchain. Here's an example of how you can connect to the Celo blockchain using `celo-sdk.js`:

```javascript
import { CeloWallet } from 'celo-sdk';

// Create a new Celo wallet
const wallet = new CeloWallet();

// Connect to the Celo blockchain
await wallet.connect();

// Check if the wallet is connected
if (wallet.isConnected()) {
  console.log('Connected to Celo blockchain');
} else {
  console.error('Failed to connect to Celo blockchain');
}
```

In this example, we created a new `CeloWallet` object and used its `connect()` method to connect to the Celo blockchain. The `isConnected()` method can be used to check if the wallet is connected to the blockchain.

Once you've connected to the Celo blockchain using `celo-sdk.js`, you can implement the logic for your DApp. Let's see an example of how you can send a transaction to invoke a function of your smart contract using `celo-sdk.js`:

```javascript
import { CeloContract, CeloWallet, ContractKit } from 'celo-sdk';

// Create a new Celo wallet
const wallet = new CeloWallet();

// Connect to the Celo blockchain
await wallet.connect();

// Create a new ContractKit instance
const kit = new ContractKit(wallet);

// Get the contract instance
const egoToken = new CeloContract(kit, contractAbi, contractAddress);

// Invoke the 'transfer' function
const tx = await egoToken.methods.transfer('0x1234567890123456789012345678901234567890', 100).send();

// Wait for the transaction to be mined
await tx.waitReceipt();

console.log('Transaction sent:', tx);
```

In the above code, we used `CeloContract` from `celo-sdk.js` to create an instance of our smart contract, and then used the `methods` property to call the `transfer` function with the desired arguments. The `send()` method sends the transaction to the Celo blockchain, and the `waitReceipt()` method waits for the transaction to be mined and the receipt to be available.

## Conclusion

That's it! You've successfully built a simple DApp on top of the Celo blockchain using Celo Composer. From here, you can explore more advanced features of Celo Composer, such as writing complex smart contracts with multiple functions, handling contract events, and interacting with other Celo protocols like the Celo Exchange Protocol or the Celo Stablecoin Protocol.
