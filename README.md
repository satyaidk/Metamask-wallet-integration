# Building dapp wallet integration

![Screenshot 2024-05-10 132925](https://github.com/satyaidk/Metamask-wallet-integration/assets/98378624/e12f84a7-b934-43bf-933d-17c960469384)

First we will create a basic react application, and then we will add a connect to Metamask button. The application will also check if MetaMask extension is installed or not. If it is not installed, the application will display a message to download the extension.

Once the account is connected, we will display the connected account address.

Let’s get to coding first. Fire your terminal and add the following instructions:

    mkdir helloreact
    cd helloreact
    npx create-react-app my-app

This will first create a helloreact folder and then my-app, a basic react application for you. Go ahead and write the following command.

    cd my-app
    npm start

This will start your server and automatically open a window for you, where you can see the default react app.

Now open the src/App.js file in VSCode. This is where we will make our edits.We will add the following dependencies in your App.js file. Go ahead and add the following imports.

    import { useEffect, useState } from "react";
    import { Contract, providers } from "ethers";

At this point, you will see an error message in your browser that ethers is not recognized. In the terminal write the following command.

    npm i ethers

This is to install ethers for your project. So what is ethers? The ethers.js library aims to be a complete and compact library for interacting with the Ethereum Blockchain and its ecosystem.

Next we will do some housekeeping stuff by using React’s useState hook to keep track

1. If the user has installed MetaMask wallet.
2. If the user has connected their account of metamask already

In the function app() section. Add the following lines:

     const [isWalletInstalled, setIsWalletInstalled] = useState(false);
    // state for keeping track of current connected account.
    const [account, setAccount] = useState(null);

    useEffect(() => {
        if (window.ethereum) {
            setIsWalletInstalled(true);
        }
    }, []);

    async function connectWallet() {
        window.ethereum
            .request({
                method: "eth_requestAccounts",
            })
            .then((accounts) => {
                setAccount(accounts[0]);
            })
            .catch((error) => {
                alert("Something went wrong");
            });
    }


Go ahead and copy paste these and I will explain the code.

1. useState is a Hook that allows you to have state variables in functional components. You pass the initial state to this function and it returns a variable with the current state value (not necessarily the initial state) and another function to update this value. We are starting with a false state for if the wallet is installed and a null state for if an account is connected.
   
2. When you call useEffect , you're telling React to run your “effect” function after flushing changes to the DOM. Effects are declared inside the component so they have access to its props and state. By default, React runs the effects after every render — including the first render.

3. MetaMask injects a global API into websites visited by its users at window.ethereum. This API allows websites to request users' Ethereum accounts, read data from blockchains the user is connected to, and suggest that the user sign messages and transactions. The function connectWallet simply does a remote procedure call to Ethereum via MetaMask. It returns a Promise that resolves to the result of the method call.

Now we will remove all the default front end elements and replace them with the following code.

      if (account === null) {
        return (
          <div className="App">
            {
              isWalletInstalled ? (
                <button onClick={connectWallet}>Connect Wallet</button>
              ) : (
                <p>Install Metamask wallet</p>
              )
            }

          </div>
        );
      }
        return (
          <div className="App">
            <p>Connected as: {account}</p>
          </div>
      );
    }

The code simply checks first if the metamask account is already connected. If connected, it shows the connected account address.

It also checks if the metamask wallet is installed. If it is not installed, we show a message to install the wallet. Else, we show a button to Connect Wallet. OnClick we trigger the connectWallet function that we have created above.

That’s all folks, we are ready to check. By now your code looks something like this.

      import logo from './logo.svg';
      import './App.css';

      import { useEffect, useState } from "react";
      import { Contract, providers } from "ethers";

      function App() {

       const [isWalletInstalled, setIsWalletInstalled] = useState(false);
       // state for keeping track of current connected account.
       const [account, setAccount] = useState(null);

       useEffect(() => {
             if (window.ethereum) {
                 setIsWalletInstalled(true);
             }
         }, []);

       async function connectWallet() {
             window.ethereum
                 .request({
                     method: "eth_requestAccounts",
                 })
                 .then((accounts) => {
                     setAccount(accounts[0]);
                 })
                 .catch((error) => {
                     alert("Something went wrong");
                 });
      }

      if (account === null) {
        return (
          <div className="App">
            {
              isWalletInstalled ? (
                <button onClick={connectWallet}>Connect Wallet</button>
              ) : (
                <p>Install Metamask wallet</p>
              )
           }

         </div>
       );
     }
       return (
         <div className="App">
           <p>Connected as: {account}</p>
        </div>
     );
    }
    export default App;

 Go ahead and run the following command in the terminal.

    npm start

Everything should run smoothly. Congrats for building this mini project and following through the instructions. :)

# Image of connected wallet Address:

![Screenshot 2024-05-10 132932](https://github.com/satyaidk/Metamask-wallet-integration/assets/98378624/6c0a2260-9116-4398-bd20-e209c7330f82)


