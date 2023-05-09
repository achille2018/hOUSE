## Title: Building a Decentralized Application using Celo Blockchain

## Table of content
- [table of content](#table-of-content)
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Step 1: Writing the Smart Contract](#step-1-writing-the-smart-contract)
- [Step 2: Compiling and Deploying the Smart Contract](#step-2-compiling-and-deploying-the-smart-contract)
- [Step 3: Building the Frontend Interface](#step-3-building-the-frontend-interface)
- [Step 4: Running the Application](#step-4-running-the-application)
    

## Introduction:
In this tutorial, we will learn how to build a decentralized application (DApp) using Celo blockchain technology. We will be using the Celo development environment to create a simple smart contract and deploy it to the Celo blockchain. Then, we will create a frontend interface using React.js to interact with the smart contract.

Celo is an open-source blockchain platform that enables the creation of decentralized applications with a focus on mobile accessibility and financial inclusion. Celo uses a Proof of Stake (PoS) consensus mechanism and has its own stablecoin called cUSD, which is pegged to the US dollar.

## Prerequisites:

Basic knowledge of JavaScript and React.js
Basic understanding of blockchain technology and smart contracts
Tools Used:

Celo Wallet Extension: to create a new account on the Celo network and transfer cUSD for gas fees
Remix IDE: to write, compile, and deploy the smart contract to the Celo network
React.js: to build the frontend interface
Step 1: Setting up the Development Environment
To get started, we need to set up our development environment. First, we need to install the Celo Wallet Extension on our browser. This extension allows us to create a new account on the Celo network, view our account balances, and transfer cUSD for gas fees.

Once the extension is installed, we can create a new account and fund it with some cUSD. This cUSD will be used to pay for the gas fees required to deploy our smart contract to the Celo network.

Next, we need to install Remix IDE, an online editor for smart contracts. Remix IDE allows us to write, compile, and deploy our smart contract to the Celo network.

Finally, we need to create a new React.js project. We can do this by running the following command in our terminal:

npx create-react-app my-dapp


## Step 1: Writing the Smart Contract
Now that our development environment is set up, we can start writing our smart contract. In this tutorial, we will create a simple smart contract that stores a single string value.

Open Remix IDE and create a new file called MyContract.sol. Then, copy and paste the following code into the file:


pragma solidity ^0.8.0;

/**
 * @title MyContract
 * @dev A simple contract to store and retrieve a string value.
 */

contract MyContract {
    string private value;
    
    /**
     * @dev Event emitted when the value of the string is changed.
     * @param newValue The new value of the string.
     */
    
    event ValueChanged(string newValue);
    
    /**
     * @dev Sets the value of the string.
     * @param _value The new value to be set.
     */
    function setValue(string memory _value) public {
        value = _value;
        emit ValueChanged(value);
    }
    /**
     * @dev Returns the value of the string.
     * @return The current value of the string.
     */
    function getValue() public view returns (string memory) {
        return value;
    }
}

This smart contract defines a private string variable called value and two functions: setValue and getValue. The setValue function takes a string parameter and sets the value of value, while the getValue function returns the current value of value.

I have also added an event which will be emitted when a new value is set

## Step 2: Compiling and Deploying the Smart Contract
To deploy the smart contract to the Celo network using Remix IDE, you can follow these steps:

1. Open Remix IDE and switch to the Solidity tab.

2. Select the `MyContract.sol` file.

3. Click on the `Compile` button to compile the smart contract. The compilation process will start, and you will be able to see the compilation status in the output console. Once the compilation is successful, you should see a green checkmark next to the file name.

4. Next, switch to the `Deploy & Run Transactions` tab.

5. Under the Environment dropdown, select `Injected Web3`. This will allow Remix IDE to connect to your Celo Wallet Extension.

6. In the `Deploy` section, select `MyContract` from the contract dropdown.

7. Click on the `Deploy` button. This will trigger a transaction that deploys the contract to the Celo network. You will be prompted to confirm the transaction in your Celo Wallet Extension.

8. Once the transaction is confirmed, Remix IDE will display the contract address and other relevant details in the `Deployed Contracts` section.

9. You can now interact with the deployed contract by calling its functions or sending transactions to it using Remix IDE or any other tool that supports web3.js and the Celo network.


Once the transaction is confirmed, you should see a message in Remix IDE indicating that the smart contract has been deployed successfully. The message will contain the contract address, which we will need in the next step.

## Step 3: Building the Frontend Interface
Now that our smart contract is deployed to the Celo network, we can build the frontend interface to interact with it.

Open the my-dapp directory in your terminal and install the following dependencies:
```
npm install @celo/contractkit web3 react-bootstrap
``` 

  ### what does the above code do?

The command `npm install @celo/contractkit web3 react-bootstrap` installs three packages in a Node.js project:

1. `@celo/contractkit`: This package provides a set of utilities for interacting with smart contracts on the Celo network. It includes a web3.js instance pre-configured to work with the Celo network, as well as several helper functions for common contract interactions.

2. `web3`: This package is a JavaScript library that provides an interface for interacting with the Ethereum blockchain and other compatible blockchains. It is used by the `@celo/contractkit` package to communicate with the Celo network.

3. `react-bootstrap`: This package provides a set of pre-built UI components for building web applications using React.js. It includes things like buttons, forms, and navigation menus, all styled according to the Bootstrap framework.

In summary, `npm install @celo/contractkit web3 react-bootstrap` installs the necessary dependencies to develop web applications that interact with smart contracts on the Celo network, using the React.js framework and the web3.js library.


npm install @celo/contractkit web3 react-bootstrap
@celo/contractkit is a library that allows us to interact with smart contracts on the Celo network. web3 is a library that allows us to connect to the Celo network using the Celo Wallet Extension. 

react-bootstrap is a library of pre-built UI components that we can use to build our interface.

Create a new file called MyContract.js in the src directory and copy and paste the following code into the file: 

```
import React, { useState } from "react";
import { Button, Container, Form } from "react-bootstrap";
import { ContractKit } from "@celo/contractkit";
import Web3 from "web3";

const web3 = new Web3(window.celo);

const kit = ContractKit.newKit("https://forno.celo.org");

const contractAddress = "YOUR_CONTRACT_ADDRESS_HERE";

const contractABI = [
  {
    inputs: [],
    name: "getValue",
    outputs: [{ internalType: "string", name: "", type: "string" }],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [{ internalType: "string", name: "_value", type: "string" }],
    name: "setValue",
    outputs: [],
    stateMutability: "nonpayable",
    type: "function",
  },
];

const myContract = new kit.web3.eth.Contract(contractABI, contractAddress);

const MyContract = () => {
  const [value, setValue] = useState("");
  const [newValue, setNewValue] = useState("");

  const handleChange = (event) => {
    setNewValue(event.target.value);
  };

  const handleSubmit = async (event) => {
    event.preventDefault();
    const accounts = await kit.web3.eth.getAccounts();
    const account = accounts[0];
    const txObject = myContract.methods.setValue(newValue);
    const tx = await kit.sendTransactionObject(txObject, {
      from: account,
      gasPrice: "1000000000",
    });
    await tx.waitReceipt();
    setValue(newValue);
    setNewValue("");
  };

  const handleGetValue = async () => {
    const value = await myContract.methods.getValue().call();
    setValue(value);
  };

   return (
    <Container>
      <h1>My Contract</h1>
      <p>Current Value: {value}</p>
      <Form onSubmit={handleSubmit}>
        <Form.Group controlId="formBasicEmail">
          <Form.Label>New Value</Form.Label>
          <Form.Control
            type="text"
            placeholder="Enter new value"
            value={newValue}
            onChange={handleChange}
          />
        </Form.Group>
        <Button variant="primary" type="submit">
          Set Value
        </Button>
      </Form>
      <Button variant="primary" onClick={handleGetValue}>
        Get Value
      </Button>
    </Container>
  );
};

export default MyContract;
```


Lets take a look under the hood and see what is going on;

1. The code imports the necessary libraries and sets up the connection to the Celo network using the Celo Wallet Extension. It also defines the contract address and ABI, which we obtained in the previous step.

2. The code defines a new instance of the `ContractKit` and a new instance of the smart contract using the contract address and ABI.

3. Next, the code defines a functional component called `MyContract`. This component uses React hooks to manage the state of the component. The state includes the current value of the smart contract and the new value that the user enters in the input field.

4. The `handleChange` function is called when the user enters a new value in the input field. It updates the state of the component with the new value.

5. The `handleSubmit` function is called when the user clicks the "Set Value" button. It first gets the user's account address using the `getAccounts` function from the `ContractKit` library. It then creates a transaction object to call the `setValue` function on the smart contract with the new value. The transaction is sent using the `sendTransactionObject` function from the `ContractKit` library. Finally, the function updates the state of the component with the new value and clears the input field.

6. The `handleGetValue` function is called when the user clicks the "Get Value" button. It calls the `getValue` function on the smart contract using the `call` function from the `ContractKit` library. It then updates the state of the component with the current value of the smart contract.

7. Finally, the `MyContract` component returns a Bootstrap container with a title, the current value of the smart contract, an input field and a button to set the new value, and a button to get the current value.


Open the App.js file in the src directory and replace the code with the following:

javascript
```
import React from "react";
import { Container } from "react-bootstrap";
import MyContract from "./MyContract";

function App() {
  return (
    <Container>
      <MyContract />
    </Container>
  );
}

export default App;
```
This code imports the MyContract component and renders it inside a Bootstrap container.

## Step 4: Running the Application

Once you have set up the React app and integrated it with the smart contract using ContractKit and web3, it's time to test your dApp. To do this, navigate to the root directory of your project in your terminal and run the following command:

```
npm start
```

This command will start the development server and automatically open your application in your default browser. Once the app is loaded, you will see a form that allows you to set and get the value of the smart contract.

To set a new value for the contract, simply enter the desired value in the input field and click on the "Set Value" button. This will trigger the handleSubmit function and send a transaction to the Celo network to set the new value for the contract.

To retrieve the current value of the smart contract, click on the "Get Value" button. This will trigger the handleGetValue function, which will call the getValue function from the smart contract using ContractKit and retrieve the current value from the Celo network.

If everything is working correctly, you should see the updated value displayed on the screen. Congratulations, you have successfully built a simple dApp using Celo blockchain technology!
