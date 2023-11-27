
If you're building a decentralized rewards platform and want to develop smart
contracts on the Ethereum blockchain, you'll need to set up a development
environment that allows you to write, test, and deploy our contracts. One
popular tool for this purpose is Hardhat. In this blog post, I'll guide you
through the process of setting up a development environment using Hardhat.

## What is Hardhat?

Hardhat is a powerful Ethereum development environment that allows you to
compile, test, and deploy smart contracts. It provides a comprehensive set of
tools and plugins that make it easy to develop, debug, and deploy our
decentralized applications (DApps). Hardhat also offers support for writing
tests and running them in a local development network or on public testnets
like Sepolia or Kovan.

Check out the official [hardhat website](https://hardhat.org/) for documentation
and tutorials.

## Prerequisites

Before you can set up our development environment using Hardhat, you'll need to
have the following prerequisites installed on our machine:

Check if prerequisites are installed by running the following commands in our
terminal:
```
$ node -v
$ npm -v
```
1. Node.js: Hardhat requires Node.js to run. You can download and install
   Node.js from the official website: https://nodejs.org.
2. npm: npm is the package manager for Node.js. It is installed along with
   Node.js, so you don't need to install it separately.

## Setting Up Your Development Environment

Now that you have the prerequisites installed, you can proceed with setting up
our development environment using Hardhat:

### Step 1: Create a New Project Directory

Start by creating a new directory for our project. Open your terminal or
command prompt and navigate to the directory where you want to create our project.
Then, run the following command to create a new directory:
```
mkdir decentralized-rewards-platform
```
Navigate into the new directory:
```
cd decentralized-rewards-platform
```
### Step 2: Initialize Your Project

Next, initialize the project by running the following command:
```
npm init -y
```
This command will create a  package.json  file in our project directory, which
will keep track of our project's dependencies and configurations.

### Step 3: Install Hardhat

Now, let's install Hardhat by running the following command:
```
npm install --save-dev hardhat
```

This will install Hardhat as a development dependency for our project.

In the same directory where you installed Hardhat, run:
```
npx hardhat init
```
<br>
[Select](Select) `Create an empty hardhat.config.js` with the arrow keys and press enter.
```
$ npx hardhat init
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

👷 Welcome to Hardhat v2.19.1 👷‍

? What do you want to do? …
  Create a JavaScript project
  Create a TypeScript project
❯ Create an empty hardhat.config.js
  Quit
```
<br>
NOTE from [Hardhat tutorial](https://hardhat.org/tutorial/creating-a-new-hardhat-project):
> When hardhat is run, it searches for the closest `hardhat.config.js` file
> starting from the current working directory. This file normally lives in the
> root of our project and an empty `hardhat.config.js` is enough for Hardhat to
> work. The entirety of our setup is contained in this file.

> Hardhat is unopinionated in terms of what tools you end up using, but it does
> come with some built-in defaults. All of which can be overridden. Most of the
> time the way to use a given tool is by consuming a plugin that integrates it
> into Hardhat.

To install recommended plugin, `@nomicfoundation/hardhat-toolbox`, which has
everything you need for developing smart contracts.
```
npm install --save-dev @nomicfoundation/hardhat-toolbox
```
<br>
### Step 4: Configure Hardhat

Only if you chose NOT to create an empty hardhat.config.js. Once Hardhat is
installed, you need to configure it for our project. Create a new file called
hardhat.config.js in our project directory and add the following configuration:
```
require("@nomiclabs/hardhat-waffle");
require("@nomicfoundation/hardhat-toolbox");

/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
solidity: "0.8.19",
};
```
This configuration specifies that you'll be using Solidity version 0.8.19 for
our smart contracts. You can change this version according to your needs.
<br>
### Step 5: Write Your Smart Contracts

Now, you're ready to start writing our smart contracts. Create a new directory
called contracts in our project directory, and create your smart contract files
inside it. For example, you can create a file called  RewardToken.sol for our
reward token contract.

```
//SPDX-License-Identifier: UNLICENSED

// Solidity files have to start with this pragma.
// It will be used by the Solidity compiler to validate its version.
pragma solidity ^0.8.0;

// This is the main building block for smart contracts.
contract RewardToken {
	//Some string type variables to identify the token.
    string public name = "My Reward Token";
    string public symbol = "MRT";
	
	// The fixed amount of tokens, stored in an unsigned integer type variable.
    uint256 public totalSupply = 1000000;

    // An address type variable is used to store ethereum accounts.
    address public owner;

    // A mapping is a key/value map. Here we store each account's balance.
    mapping(address => uint256) balances;

    // The Transfer event helps off-chain applications understand
    // what happens within our contract.
    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    /**
     * Contract initialization.
     */
    constructor() {
        // The totalSupply is assigned to the transaction sender, which is the
        // account that is deploying the contract.
        balances[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    /**
     * A function to transfer tokens.
     *
     * The `external` modifier makes a function *only* callable from *outside*
     * the contract.
     */
    function transfer(address to, uint256 amount) external {
        // Check if the transaction sender has enough tokens.
        // If `require`'s first argument evaluates to `false` then the
        // transaction will revert.
        require(balances[msg.sender] >= amount, "Not enough tokens");

        // Transfer the amount.
        balances[msg.sender] -= amount;
        balances[to] += amount;

        // Notify off-chain applications of the transfer.
        emit Transfer(msg.sender, to, amount);
    }

    /**
     * Read only function to retrieve the token balance of a given account.
     *
     * The `view` modifier indicates that it doesn't modify the contract's
     * state, which allows us to call it without executing a transaction.
     */
    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }
}
```

### Step 6: Compile Your Smart Contracts

To compile our smart contracts run `npx hardhat compile` in your terminal. The
`compile` task is one of the built-in tasks.
```
$ npx hardhat compile
Compiling 1 file with 0.8.19
Compilation finished successfully
```
This command will compile our smart contracts and generate the corresponding
artifacts in a  build  directory.

### Step 7: Write Tests

Writing tests for our smart contracts is crucial to ensure their correctness and
robustness. Create a new directory called  `test`  in our project directory, and create
our test files inside it. For example, you can create a file called
`RewardToken.js`  to test our reward token contract.

Example test file:
```
const { expect } = require("chai");

describe("RewardToken contract", function () {
  it("Deployment should assign the total supply of tokens to the owner", async function () {
    const [owner] = await ethers.getSigners();

    const hardhatToken = await ethers.deployContract("Token");

    const ownerBalance = await hardhatToken.balanceOf(owner.address);
    expect(await hardhatToken.totalSupply()).to.equal(ownerBalance);
  });
});
```

### Step 8: Run Tests

To run our tests, run `npx hardhat test`. You should see the following output:
```
$ npx hardhat test

  RewardToken contract
  ✓ Deployment should assign the total supply of tokens to the owner (105ms)
  
  1 passing (119ms)
```

### Step 9: Deploy Your Smart Contracts

Once our smart contracts are tested, we can deploy them to the Ethereum network.
The "mainnet" network deals with real money, while there are other separate
networks for testing purposes like (Sepolia, Kovan, Rinkeby, etc.).

To do this, let's create a new directory `scripts` inside the project root's
directory, and paste the following into a `deploy.js` file in that directory:
```
async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying contracts with the account:", deployer.address);

  const token = await ethers.deployContract("Token");

  console.log("Token address:", await token.getAddress());
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

To tell Hardhat to connect to a specific Ethereum network, you can use the
`--network` parameter when running any task, like this:
```
npx hardhat run scripts/deploy.js --network <network-name>
```
<br>
To deploy to a remote network such as mainnet or testnet, you need to add a
network entry to our `hardhat.config.js` file. We'll use Sepolia for this
example, but you can add any network similarly:
```
require("@nomicfoundation/hardhat-toolbox");

// Go to https://infura.io, sign up, create a new API key
// in its dashboard, and replace "KEY" with it
const INFURA_API_KEY = "KEY";

// Replace this private key with our Sepolia account private key
// To export our private key from Coinbase Wallet, go to
// Settings > Developer Settings > Show private key
// To export our private key from Metamask, open Metamask and
// go to Account Details > Export Private Key
// Beware: NEVER put real Ether into testing accounts
const SEPOLIA_PRIVATE_KEY = "YOUR SEPOLIA PRIVATE KEY";

module.exports = {
  solidity: "0.8.19",
  networks: {
    sepolia: {
      url: `https://sepolia.infura.io/v3/${INFURA_API_KEY}`,
      accounts: [SEPOLIA_PRIVATE_KEY]
    }
  }
};
```
We're using [Infura](https://infura.io/), but pointing `url` to any Ethereum
node or gateway.

Finally, run:
```
npx hardhat run scripts/deploy.js --network sepolia
```

**Congratulations!** You have successfully set up a development environment for our
decentralized rewards platform using Hardhat. You can now start building and testing
our smart contracts, and deploy them to a blockchain network.

## Conclusion

Setting up a development environment for our decentralized rewards platform using
Hardhat is essential for efficient smart contract development
