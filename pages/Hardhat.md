- ## Installation
- npm init --yes
  npm install --save-dev hardhat
  npx hardhat init
  mkdir contracts
  mkdir scripts
- Écrire le `Contract.sol`
- ```solidity
  //SPDX-License-Identifier: UNLICENSED
  
  pragma solidity ^0.8.0;
  
  [...]
  ```
- Compiler pour vérifier qu'il n'y ait pas d'erreurs
- `npx hardhat compile`
- ## Déploiment
- ```bash
  npm install dotenv --save
  ```
- Mettre `.env` dans `.gitignore`
- ```.env
  ALCHEMY_API_KEY = "API-KEY"
  SEPOLIA_PRIVATE_KEY = "metamask_private_key"
  ```
- Dans `hardhat.config.js`
- ```javascript
  require("@nomicfoundation/hardhat-toolbox");
  require('dotenv').config();
  
  const { ALCHEMY_API_KEY, SEPOLIA_PRIVATE_KEY} = process.env;
  
  module.exports = {
    solidity: "0.8.24",
    networks: {
      sepolia: {
        url: `https://eth-sepolia.g.alchemy.com/v2/${ALCHEMY_API_KEY}`,
        accounts: [SEPOLIA_PRIVATE_KEY]
      }
    }
  };
  ```
- ### Sans ingnition
- Dans `scripts/deploy.js`
- ```javascript
  async function main() {
      const [deployer] = await ethers.getSigners();
  
      console.log("Deploying contracts with the account: ", deployer.address);
  
      const HelloWoldFactory = await ethers.getContractFactory("HelloWorld");
  
      const helloWorld = await HelloWoldFactory.deploy("Hello World!");
  
      console.log("Contract address: ", await helloWorld.getAddress());
  }
  
  main()
      .then(() => process.exit(0))
      .catch((error) => {
          console.error(error);
          process.exit(1);
      })
  ```
- Déployer
- `npx hardhat run scripts/deploy.js --network sepolia`
- ### Avec ignition
- `mkdir -p ignition/modules`
- `touch ignition/modules/HelloWorld.js`
- ```javascript
  // .ignition/modukes/HelloWorld.js
  
  const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");
  
  const message = "Hello World!";
  
  const HelloWorldModule = buildModule("HelloWorldModule", (m) => {
    const helloWorld = m.contract("HelloWorld", [message]);
  
    return { helloWorld };
  });
  
  module.exports = HelloWorldModule;
  ```
- `npx hardhat ignition deploy ./ignition/modules/HelloWorld.js --network sepolia`
- ## sources
- https://www.youtube.com/watch?v=g73EGNKatDw&list=PLMj8NvODurfGgDJG-qQWyKtqTxJyRGI0i
- https://hardhat.org/tutorial/deploying-to-a-live-network