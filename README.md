# Web3 Project Setup

This document provides a step-by-step guide for setting up the Web3 portion of this project. It covers initializing a Hardhat project with TypeScript, installing the necessary dependencies, configuring your Avalanche wallet, creating required configuration files, compiling and deploying your smart contract, and finally integrating the deployed contract with the frontend.

---

## Table of Contents

1. Project Initialization
2. Installing Dependencies
3. Configure Your Avalanche Wallet
4. File Creation and Configuration
   - Creating the `.env` File
   - Creating the `hardhat.config.ts` File
   - Creating the Deployment Script
   - Creating the Smart Contract File
5. Compilation and Deployment
6. Frontend Integration
7. Additional Notes

---

**1. Project Initialization**

1.  **Navigate to the Project Directory:** Open your terminal and run: `cd web3`
2.  **Initialize Hardhat:** Run the following command to start the Hardhat project creation wizard: `npx hardhat`. When prompted: Choose to create a new project. Select TypeScript as your project type. Press Enter to accept the default settings.

**2. Installing Dependencies**

Install both the runtime and development dependencies as follows:

**Runtime Dependencies:**
`npm install @openzeppelin/contracts dotenv @nomiclabs/hardhat-ethers`

**Development Dependencies (Hardhat Core & Toolbox):**
`npm install --save-dev hardhat@^2.12.0 @nomicfoundation/hardhat-toolbox@^2.0.0`

**Note:** If you experience dependency conflicts, try adding the `--legacy-peer-deps` flag to your install commands: `npm install <package-name> --legacy-peer-deps`

**3. Configure Your Avalanche Wallet**

### Install and Enable Testnet Mode in Core Wallet

1.  **Install Core Wallet:** Download and install the [Core wallet extension](https://www.google.com/search?q=https://core.app/download/) (an Avalanche-compatible alternative to MetaMask).
2.  **Enable Testnet Mode:** Open the Core wallet extension. Click the hamburger menu (top left). Go to Advanced and turn on Testnet Mode.

### Fund Your Wallet:

Visit the [AVAX Faucet](https://faucet.avax.network/) to fund your wallet with test AVAX.

**4. File Creation and Configuration**

### Creating the `.env` File

1.  **Create the File:** In your project root (inside the `web3` directory), create a file named `.env`. On Unix-like systems: `touch .env`. On Windows: Create the file manually using your code editor.
2.  **Add Your Private Key:** Open `.env` and add your private key in the following format: `PRIVATE_KEY=your_c_chain_private_key_here`
3.  **Retrieving Your C-Chain Private Key:** Open the Core wallet extension. Click the hamburger menu (top left) and select Security and Privacy. Click Show Recovery Phrase and enter your password. Copy the recovery phrase. Visit [wallet.avax.network](https://wallet.avax.network/) and click Access Wallet. Choose Mnemonic Key Phrase and paste the recovery phrase. In the sidebar, click Manage Keys then select View C-Chain Private Key. Copy the key and paste it into your `.env` file.

### Creating the `hardhat.config.ts` File

1.  **Create the File:** In the project root directory, create a new file named `hardhat.config.ts`.
2.  **Add the Configuration:** Open `hardhat.config.ts` and paste the following content (adjust settings as needed):
    ```typescript
    import { HardhatUserConfig } from "hardhat/config";
    import "@nomicfoundation/hardhat-toolbox";
    import * as dotenv from "dotenv";

    dotenv.config();

    const config: HardhatUserConfig = {
      solidity: "0.8.4",
      defaultNetwork: "fuji",
      networks: {
        fuji: {
          url: "[https://api.avax-test.network/ext/bc/C/rpc](https://api.avax-test.network/ext/bc/C/rpc)",
          accounts: [process.env.PRIVATE_KEY || ""]
        }
      }
    };

    export default config;
    ```

### Creating the Deployment Script

1.  **Create the File:** Inside the `scripts` folder, create a new file named `deploy.ts`.
2.  **Add the Deployment Code:** Open `deploy.ts` and paste the following content:
    ```typescript
    import { ethers } from "hardhat";

    async function main() {
      const AvaxGodsFactory = await ethers.getContractFactory("AvaxGods");
      const contract = await AvaxGodsFactory.deploy();
      await contract.deployed();
      console.log("AvaxGods deployed to:", contract.address);
    }

    main().catch((error) => {
      console.error(error);
      process.exitCode = 1;
    });
    ```

### Creating the Smart Contract File

1.  **Create the File:** In the `contracts` folder, create a new file named `AvaxGods.sol`.
2.  **Add the Smart Contract Code:** Copy and paste your smart contract code (from the provided GitHub gist or your own version) into `AvaxGods.sol`.

**5. Compilation and Deployment**

### Compile the Smart Contract

Run the following command in your terminal to compile your contract:
`npx hardhat compile`

### Deploy to the Fuji Test Network

Deploy the contract by running:
`npx hardhat run scripts/deploy.ts --network fuji`

After deployment, the terminal will display the address of your deployed contract.

**6. Frontend Integration**

1.  **Transfer the Contract Artifact:** Locate the artifact file at `/artifacts/contracts/AvaxGods.sol/AvaxGods.json`. Copy or move this file to the `/contract` folder in your frontend project.
2.  **Update the Contract Address:** Open `/contract/index.js` (or the appropriate file) in your frontend project. Replace the placeholder contract address with the address displayed during deployment.

**7. Additional Notes**

### Dependency Conflicts:

If you experience dependency conflicts during installation, try using:
`npm install <package-name> --legacy-peer-deps`

### Documentation and Resources:

  * [Hardhat Documentation](https://hardhat.org/docs)
  * [Avalanche Documentation](https://www.google.com/search?q=https://docs.avax.network/)

-----
