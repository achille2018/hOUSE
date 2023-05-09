# Building a Decentralized Application using The Celo Blockchain-

## Table of Contents:
- [Building a Decentralized Application using The Celo Blockchain](#building-a-decentralized-application-using-the-celo-blockchain)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Pre-requisites](#pre-requisites)
  - [Tools Required](#Tools-Required)
  - [Let's Get Started](#let's-get-started)
    - [Step 1: Setting up the Development Environment](#step-1-setting-up-the-development-environment)
    - [Step 2: Writing the Smart Contract](#step-2-writing-the-smart-contract)
    - [Step 3: Compiling and Deploying the Smart Contract](#step-3-compiling-and-deploying-the-smart-contract)
    - [Step 4: Building the Frontend Interface](#step-4-building-the-frontend-interface)
    - [Step 5: Running the Application](#step-5-running-the-application)
  - [Conclusion](#conclusion)

## Introduction:

In this tutorial, we will learn how to build a decentralized application (DApp) using Celo blockchain technology. We will be using the Celo development environment to create a simple Smart Contract and deploy it to the Celo blockchain. Then, we will create a frontend interface using [React.js](https://react-cn.github.io/react/downloads.html) to interact with the Smart Contract.

Celo is an open-source blockchain platform that enables the creation of decentralized applications (DApps) with a focus on mobile accessibility and financial inclusion. Celo uses a Proof-of-Stake (PoS) consensus mechanism and has its own stablecoin called cUSD, which is pegged to the US dollar.

## Pre-requisites:

1. Knowledge of JavaScript and [React.js](https://react-cn.github.io/react/downloads.html).
2. Understanding of Blockchain Technology and Smart Contracts.

## Tools Required:

1. [Celo Wallet Extension](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en)
2. [Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.18+commit.87f61d96.js)
3. [React.js](https://react-cn.github.io/react/downloads.html)

## Let's Get Started!

## Step 1: Setting up the Development Environment-

To get started, we need to set up our development environment. Firstly, we need to install the [Celo Wallet Extension](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en) on our browser. This extension will allow us to create a new account on the Celo network, view our account balances and transfer cUSD for gas fees.

Once the extension is installed, we can create a new account and fund it with some cUSD. This cUSD will be used to pay for the gas fees required to deploy our Smart Contract to the Celo network.

Next, we need to install [Remix IDE](https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.18+commit.87f61d96.js) which is an online editor for Smart Contracts. Remix IDE allows us to write, compile and deploy our Smart Contract to the Celo network.

Finally, we need to create a new React.js project. We can do this by running the following command in our terminal:
```
npx create-react-app my-dapp
```

## Step 2: Writing the Smart Contract-

Now that our development environment is set up, we can start writing our Smart Contract. In this tutorial, we will create a simple Smart Contract that stores a single string value.

Open Remix IDE and create a new file called "MyContract.sol". Then, copy and paste the following code into the file:

```
// SPDX-License-Identifier: MIT
// This contract demonstrates a simple string storage contract with error handling
pragma solidity ^0.8.0;

contract MyContract {
    // Private string variable to store the value
    string private value;

    // Event that is emitted whenever the value is set
    event ValueSet(string value);

    // Function to set the value
    function setValue(string memory _value) public {
        // Check that the value is not empty
        require(bytes(_value).length > 0, "Value cannot be empty");

        // Set the value and emit the ValueSet event
        value = _value;
        emit ValueSet(value);
    }

    // Function to get the value
    function getValue() public view returns (string memory) {
        return value;
    }
}

```

The above Smart Contract defines a private string variable called value and two functions:
1. `setValue` function: takes a string parameter and sets the value of value.
2. `getValue` function: returns the current value of value.

## Step 3: Compiling and Deploying the Smart Contract-

Now that we have written our Smart Contract, we need to compile and deploy it to the Celo network. Remix IDE allows us to do this easily.

In Remix IDE, switch to the Solidity tab and select "MyContract.sol". Then, click on the "Compile" button to compile the Smart Contract. Once the compilation is successful, you should see a green checkmark next to the file name.

Next, switch to the "Deploy & Run Transactions" tab. Under the Environment dropdown, select "Injected Web3". This will allow us to connect Remix IDE to our Celo Wallet Extension.

In the Deploy section, select "MyContract" from the contract dropdown. Then, click on the Deploy button and you will be prompted to confirm the transaction in your Celo Wallet Extension.

Once the transaction is confirmed, you should see a message in Remix IDE indicating that the Smart Contract has been deployed successfully. The message will contain the contract address, which we will need in the next step.

## Step 4: Building the Frontend Interface-

Now that our Smart Contract is deployed to the Celo network, we can build the frontend interface to interact with it.

Open the my-dapp directory in your terminal and install the following dependencies:
```
npm install @celo/contractkit web3 react-bootstrap
```
Once the transaction is confirmed, you should see a message in Remix IDE indicating that the Smart Contract has been deployed successfully. The message will contain the contract address, which we will need in the next step.

Step 5: Building the Frontend Interface-

Now that our Smart Contract is deployed to the Celo network, we can build the frontend interface to interact with it.

Open the `my-dapp directory` in your terminal and install the following dependencies:
```
bash
```
```
npm install @celo/contractkit web3 react-bootstrap
```
The above dependencies simply means the following:

1. **@celo/contractkit** is a library that allows us to interact with the Smart Contracts on the Celo network. 
2. **web3** is a library that allows us to connect to the Celo network using the Celo Wallet Extension. 
3. **react-bootstrap** is a library of pre-built UI components that we can use to build our interface.

Create a new file called "MyContract.js" in the src directory and copy & paste the following code into the file: 

```
import React, { useState } from "react";
import { Button, Container, Form, Spinner } from "react-bootstrap";
import { ContractKit } from "@celo/contractkit";
import Web3 from "web3";

const web3 = new Web3(window.celo);

const kit = ContractKit.newKit("https://forno.celo.org");

const CONTRACT_ADDRESS = "YOUR_CONTRACT_ADDRESS_HERE";

const CONTRACT_ABI = [
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

const myContract = new kit.web3.eth.Contract(CONTRACT_ABI, CONTRACT_ADDRESS);

const MyContract = () => {
  const [value, setValue] = useState("");
  const [newValue, setNewValue] = useState("");
  const [loading, setLoading] = useState(false);

  const handleChange = (event) => {
    setNewValue(event.target.value);
  };

  const handleSubmit = async (event) => {
    event.preventDefault();
    if (!newValue) {
      alert("Please enter a new value.");
      return;
    }
    setLoading(true);
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
    setLoading(false);
  };

  const handleGetValue = async () => {
    setLoading(true);
    const value = await myContract.methods.getValue().call();
    setValue(value);
    setLoading(false);
  };

  return (
    <Container>
      <h1>My Contract</h1>
      <p>Current Value: {loading ? <Spinner animation="border" size="sm" /> : value}</p>
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
          {loading ? <Spinner animation="border" size="sm" /> : "Set Value"}
        </Button>
      </Form>
      <Button variant="primary" onClick={handleGetValue}>
        {loading ? <Spinner animation="border" size="sm" /> : "Get Value"}
      </Button>
    </Container>
  );
};

export default MyContract;

```
This code imports the necessary libraries and sets up the connection to the Celo network using the Celo Wallet Extension. It also defines the contract address and ABI, which we obtained in the previous step.

The code also defines a new instance of the `ContractKit` and a new instance of the Smart Contract using the contract address and ABI.

Next, the code defines a functional component called `MyContract`. This component uses React hooks to manage the state of the component. The state includes the current value of the Smart Contract and the new value that the user enters in the input field.

The `handleChange` function is called when the user enters a new value in the input field. It updates the state of the component with the new value.

The `handleSubmit` function is called when the user clicks the "Set Value" button. It first gets the user's account address using the `getAccounts` function from the `ContractKit` library. It then creates a transaction object to call the `setValue` function on the Smart Contract with the new value. The transaction is sent using the `sendTransactionObject` function from the `ContractKit` library. The function then, updates the state of the component with the new value and clears the input field.

The `handleGetValue` function is called when the user clicks the "Get Value" button. It calls the `getValue` function on the Smart contract using the call function from the `ContractKit` library. It then updates the state of the component with the current value of the Smart Contract.

Lastly, the `MyContract` component returns a Bootstrap container with a title, the current value of the Smart Contract, an input field & a button to set the new value and a button to get the current value.

Open the "App.js" file in the src directory and replace the code with the following:  
```
// Importing the necessary modules from react-bootstrap and MyContract component
import React from "react";
import { Container } from "react-bootstrap";
import MyContract from "./MyContract";

// Creating the main App component
function App() {
  // The component returns a Container with the MyContract component inside it
  return (
    <Container>
      <MyContract />
    </Container>
  );
}

// Exporting the App component
export default App;

```
This code imports the `MyContract` component and renders it inside a Bootstrap container.

## Step 5: Running the Application-

Now that we have built our Smart Contract and frontend interface, wherein we can run our application to see it in action!!

In your terminal, navigate to the "my-dapp" directory and run the following command: 
```
npm start
```
This command will start the development server and open the application in your default browser.

You should see a form that allows you to set and get the value of the Smart Contract. Enter a new value in the input field and click the "Set Value" button to set the new value. Click the "Get Value" button to retrieve the current value of the Smart Contract.

Congratulations!! You have built a DApp using the Celo blockchain technology.   

## Conclusion:

Therefore, by leveraging Celo's Smart Contract capabilities and decentralized infrastructure, developers can build applications that are transparent, trustless and highly resilient to attacks. Additionally, Celo's focus on financial inclusion and mobile-first approach make it an attractive platform for developing applications that can empower communities and individuals who have limited access to traditional banking services.

Overall, building a decentralized application (DApp) using the Celo blockchain can provide developers with a unique opportunity to create innovative applications that can transform industries and improve the lives of people around the world.
