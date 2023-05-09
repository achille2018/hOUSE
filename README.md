# Title: Building a Decentralized Application using Celo Blockchain

## Introduction:
In this tutorial, we will learn how to build a decentralized application (DApp) using Celo blockchain technology. We will be using the Celo development environment to create a simple smart contract and deploy it to the Celo blockchain. Then, we will create a frontend interface using React.js to interact with the smart contract.

Celo is an open-source blockchain platform that enables the creation of decentralized applications with a focus on mobile accessibility and financial inclusion. Celo uses a Proof of Stake (PoS) consensus mechanism and has its own stablecoin called cUSD, which is pegged to the US dollar.

## Table of content
- Building a Decentralized Application using Celo Blockchain
  - [Table of content](#table-of-content)
  - [Introduction](#introduction)
  - [Prerequisites](#prerequisites)
  - [Tutorial](#tutorial)
    - [Step 1: Writing the Smart Contract](#step-2-writing-the-smart-contract)
    - [Step 2: Compiling and Deploying the Smart Contract](#step-2-compiling-and-deploying-the-smart-contract)
    - [Step 3: Compiling and Deploying the Smart Contract](#step-3-compiling-and-deploying-the-smart-Contract)
    - [Step 4: Building the Frontend Interface](#step-4-building-the-frontend-interface)
    - [Step 5: Running the Application](#step-5-running-the-application)

## Prerequisites:

- Basic knowledge of JavaScript and React.js
- Basic understanding of blockchain technology and smart contracts
- Tools Used:
   - Celo Wallet Extension: to create a new account on the Celo network and transfer cUSD for gas fees
   - Remix IDE: to write, compile, and deploy the smart contract to the Celo network
   - React.js: to build the frontend interface

## Step 1: Setting up the Development Environment
To get started, we need to set up our development environment. First, we need to install the Celo Wallet Extension on our browser. This extension allows us to create a new account on the Celo network, view our account balances, and transfer cUSD for gas fees.

Once the extension is installed, we can create a new account and fund it with some cUSD. This cUSD will be used to pay for the gas fees required to deploy our smart contract to the Celo network.

Next, we need to install Remix IDE, an online editor for smart contracts. Remix IDE allows us to write, compile, and deploy our smart contract to the Celo network.

Finally, we need to create a new React.js project. We can do this by running the following command in our terminal:

npx create-react-app my-dapp


## Step 2: Writing the Smart Contract
Now that our development environment is set up, we can start writing our smart contract. In this tutorial, we will create a simple smart contract that stores a single string value.

Open Remix IDE and create a new file called MyContract.sol. Then, copy and paste the following code into the file:

```solidity
pragma solidity ^0.8.0;

contract MyContract {
    string private value;

    function setValue(string memory _value) public {
        value = _value;
    }

    function getValue() public view returns (string memory) {
        return value;
    }
}
```

This smart contract defines a private string variable called value and two functions: setValue and getValue. The setValue function takes a string parameter and sets the value of value, while the getValue function returns the current value of value.

## Step 3: Compiling and Deploying the Smart Contract
Now that we have written our smart contract, we need to compile and deploy it to the Celo network. Remix IDE allows us to do this easily.

In Remix IDE, switch to the Solidity tab and select MyContract.sol. Then, click on the Compile button to compile the smart contract. Once the compilation is successful, you should see a green checkmark next to the file name.

Next, switch to the Deploy & Run Transactions tab. Under the Environment dropdown, select Injected Web3. This will allow us to connect Remix IDE to our Celo Wallet Extension.

In the Deploy section, select MyContract from the contract dropdown. Then, click on the Deploy button. You will be prompted to confirm the transaction in your Celo Wallet Extension.



Once the transaction is confirmed, you should see a message in Remix IDE indicating that the smart contract has been deployed successfully. The message will contain the contract address, which we will need in the next step.

## Step 4: Building the Frontend Interface
Now that our smart contract is deployed to the Celo network, we can build the frontend interface to interact with it.

Open the my-dapp directory in your terminal and install the following dependencies:
```
npm install @celo/contractkit web3 react-bootstrap
```

Once the transaction is confirmed, you should see a message in Remix IDE indicating that the smart contract has been deployed successfully. The message will contain the contract address, which we will need in the next step.

Step 4: Building the Frontend Interface
Now that our smart contract is deployed to the Celo network, we can build the frontend interface to interact with it.

Open the my-dapp directory in your terminal and install the following dependencies:

```
npm install @celo/contractkit web3 react-bootstrap
```
@celo/contractkit is a library that allows us to interact with smart contracts on the Celo network. web3 is a library that allows us to connect to the Celo network using the Celo Wallet Extension. react-bootstrap is a library of pre-built UI components that we can use to build our interface.

Create a new file called MyContract.js in the src directory and copy and paste the following code into the file: 

```javascript
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
**

This code imports the necessary libraries and sets up the connection to the Celo network using the Celo Wallet Extension. It also defines the contract address and ABI, which we obtained in the previous step.

code also defines a new instance of the ContractKit and a new instance of the smart contract using the contract address and ABI.

Next, the code defines a functional component called MyContract. This component uses React hooks to manage the state of the component. The state includes the current value of the smart contract and the new value that the user enters in the input field.

The handleChange function is called when the user enters a new value in the input field. It updates the state of the component with the new value.

The handleSubmit function is called when the user clicks the "Set Value" button. It first gets the user's account address using the getAccounts function from the ContractKit library. It then creates a transaction object to call the setValue function on the smart contract with the new value. The transaction is sent using the sendTransactionObject function from the ContractKit library. Finally, the function updates the state of the component with the new value and clears the input field.

The handleGetValue function is called when the user clicks the "Get Value" button. It calls the getValue function on the smart contract using the call function from the ContractKit library. It then updates the state of the component with the current value of the smart contract.

Finally, the MyContract component returns a Bootstrap container with a title, the current value of the smart contract, an input field and a button to set the new value, and a button to get the current value.

Open the App.js file in the src directory and replace the code with the following:  
```javascript
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
code also defines a new instance of the ContractKit and a new instance of the smart contract using the contract address and ABI.

Next, the code defines a functional component called MyContract. This component uses React hooks to manage the state of the component. The state includes the current value of the smart contract and the new value that the user enters in the input field.

The handleChange function is called when the user enters a new value in the input field. It updates the state of the component with the new value.

The handleSubmit function is called when the user clicks the "Set Value" button. It first gets the user's account address using the getAccounts function from the ContractKit library. It then creates a transaction object to call the setValue function on the smart contract with the new value. The transaction is sent using the sendTransactionObject function from the ContractKit library. Finally, the function updates the state of the component with the new value and clears the input field.

The handleGetValue function is called when the user clicks the "Get Value" button. It calls the getValue function on the smart contract using the call function from the ContractKit library. It then updates the state of the component with the current value of the smart contract.

Finally, the MyContract component returns a Bootstrap container with a title, the current value of the smart contract, an input field and a button to set the new value, and a button to get the current value.

Open the App.js file in the src directory and replace the code with the following:


```javascript
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

## Step 5: Running the Application
Now that we have built our smart contract and frontend interface, we can run our application to see it in action.

In your terminal, navigate to the my-dapp directory and run the following command: 

```
npm start
```

code also defines a new instance of the ContractKit and a new instance of the smart contract using the contract address and ABI.

Next, the code defines a functional component called MyContract. This component uses React hooks to manage the state of the component. The state includes the current value of the smart contract and the new value that the user enters in the input field.

The handleChange function is called when the user enters a new value in the input field. It updates the state of the component with the new value.

The handleSubmit function is called when the user clicks the "Set Value" button. It first gets the user's account address using the getAccounts function from the ContractKit library. It then creates a transaction object to call the setValue function on the smart contract with the new value. The transaction is sent using the sendTransactionObject function from the ContractKit library. Finally, the function updates the state of the component with the new value and clears the input field.

The handleGetValue function is called when the user clicks the "Get Value" button. It calls the getValue function on the smart contract using the call function from the ContractKit library. It then updates the state of the component with the current value of the smart contract.

Finally, the MyContract component returns a Bootstrap container with a title, the current value of the smart contract, an input field and a button to set the new value, and a button to get the current value.

Open the App.js file in the src directory and replace the code with the following:

```javascript
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
This code imports the MyContract component and renders it inside a Bootstrap container.
```

Now that we have built our smart contract and frontend interface, we can run our application to see it in action.

In your terminal, navigate to the my-dapp directory and run the following command:

sql
```
npm start
```
This command will start the development server and open the application in your default browser.

You should see a form that allows you to set and get the value of the smart contract. Enter a new value in the input field and click the "Set Value" button to set the new value. Click the "Get Value" button to retrieve the current value of the smart contract.

Congratulations! You have built a simple dApp using Celo blockchain technology.   
