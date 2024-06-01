# MODULE-2-
source codes for module 2

Smart Contract Management
This program is one that integrates a Smart Contract that allows connecting of wallets with Metamask, checking of balance, depositing, and withdrawing.

Description
This smart contract provides a simplified way of dealing with financial transactions on blockchain systems. It allows users to deposit, withdraw, and check their balance, providing easy and transparent control and access to their funds.

Getting Started
Installing
You can use VScode to run this program. Then you must copy the link of the starter template wchich you will be using when cloning it in the VScode terminal by typing "git clone" and the link you copied.

After cloning, you also need to install all the dependencies needed which can be done by typing "npm i" in the terminal, as well as the "npx hardhat run" on a different terminal. After that, you must also type "npm run dev" to launch the frontend in the first terminal.

Executing program
save the files
open localhost:3000 in the browser
import { useState, useEffect } from "react";
import { ethers } from "ethers";
import atm_abi from "../artifacts/contracts/Assessment.sol/Assessment.json";

export default function HomePage() {
  const [ethWallet, setEthWallet] = useState(undefined);
  const [account, setAccount] = useState(undefined);
  const [atm, setATM] = useState(undefined);
  const [balance, setBalance] = useState(undefined);

  const contractAddress = "0x5FbDB2315678afecb367f032d93F642f64180aa3";
  const atmABI = atm_abi.abi;

  const getWallet = async () => {
    if (window.ethereum) {
      setEthWallet(window.ethereum);
    }

    if (ethWallet) {
      const account = await ethWallet.request({ method: "eth_accounts" });
      handleAccount(account);
    }
  };

  const handleAccount = (account) => {
    if (account) {
      console.log("Account connected: ", account);
      setAccount(account);
    } else {
      console.log("No account found");
    }
  };

  const connectAccount = async () => {
    if (!ethWallet) {
      alert("MetaMask wallet is required to connect");
      return;
    }

    const accounts = await ethWallet.request({ method: "eth_requestAccounts" });
    handleAccount(accounts);

    // once wallet is set we can get a reference to our deployed contract
    getATMContract();
  };

  const getATMContract = () => {
    const provider = new ethers.providers.Web3Provider(ethWallet);
    const signer = provider.getSigner();
    const atmContract = new ethers.Contract(contractAddress, atmABI, signer);

    setATM(atmContract);
  };

  const getBalance = async () => {
    if (atm) {
      setBalance((await atm.getBalance()).toNumber());
    }
  };

  // const deposit = async () => {
  //   if (atm) {
  //     let tx = await atm.deposit(1);
  //     await tx.wait();
  //     getBalance();
  //   }
  // };

  // const withdraw = async () => {
  //   if (atm) {
  //     let tx = await atm.withdraw(1);
  //     await tx.wait();
  //     getBalance();
  //   }
  // };

  const DepositButton = async () => {
    const userInput = window.prompt(
      "Please enter the amount you want to deposit:",
      ""
    );
    if (atm) {
      let tx = await atm.deposit(userInput);
      await tx.wait();
      getBalance();
    }
  };

  const WithdrawButton = async () => {
    const userInput = window.prompt(
      "Please enter the amount you want to withdraw:",
      ""
    );
    if (atm) {
      let tx = await atm.withdraw(userInput);
      await tx.wait();
      getBalance();
    }
  };

  const checkBalance = () => {
    window.alert("Your Balance: " + balance);
  };

  const initUser = () => {
    // Check to see if user has Metamask
    if (!ethWallet) {
      return <p>Please install Metamask in order to use this ATM.</p>;
    }

    // Check to see if user is connected. If not, connect to their account
    if (!account) {
      return (
        <button onClick={connectAccount}>
          Please connect your Metamask wallet
        </button>
      );
    }

    if (balance == undefined) {
      getBalance();
    }

    return (
      <div>
        <p>Your Account: {account}</p>
        <button onClick={checkBalance}>Check balance</button>
        <button onClick={DepositButton}>Deposit</button>
        <button onClick={WithdrawButton}>Withdraw</button>
      </div>
    );
  };

  useEffect(() => {
    getWallet();
  }, []);

  return (
    <main className="container">
      <header>
        <h1>Welcome back!</h1>
      </header>
      {initUser()}
      <style jsx>
        {`
          .container {
            text-align: center;
          }
        `}
      </style>
    </main>
  );
}
Help
Make sure to install all the packages needed to avoid problems.


Authors
Kinu (keenufestin@gmail.com)

License
This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details
