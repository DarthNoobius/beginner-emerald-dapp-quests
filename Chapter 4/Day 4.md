# Quests
1. I deployed a contract called SimpleTest to an account with an address of 0x6c0d53c676256e8c. I want you to make a button that, when clicked, sends a transaction to change the number variable from that contract. If you're curious, you can see the contract here: https://flow-view-source.com/testnet/account/0x6c0d53c676256e8c/contract/SimpleTest

2. Immediately after you send the transaction, wait for the transaction to be "Sealed" just like we did today. Then, call a script to read the number from the contract. Console log the result.

![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%204/Images/Day%204%20console.png)
console.png
```javascript
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import Nav from "../components/Nav.jsx"
import { useState, useEffect } from 'react';
import * as fcl from "@onflow/fcl"


export default function Home() {
  const [greeting, setGreeting] = useState('');
  const [newGreeting, setNewGreeting] = useState('');
  const [number, setNumber] = useState('');
  const [newNumber, setnewNumber] = useState('');
  const [txStatusGreeting, settxStatusGreeting] = useState('Run New Greeting Transaction');
  const [txStatusNumber, settxStatusNumber] = useState('Run New Number Transaction');

  async function runNewGreetingTransaction() {
    const transactionId = await fcl.mutate({
      cadence: `
      import HelloWorld from 0x8e56d24cc7f80589

      transaction(myNewGreeting: String) {

        prepare(signer: AuthAccount) {}

        execute {
          HelloWorld.changeGreeting(newGreeting: myNewGreeting)
        }
      }
      `,
      args: (arg, t) => [
        arg(newGreeting, t.String)
      ], // ARGUMENTS GO IN HERE
      proposer: fcl.authz, // PROPOSER GOES HERE
      payer: fcl.authz, // PAYER GOES HERE
      authorizations: [fcl.authz], // AUTHORIZATIONS GO HERE
      limit: 999 // GAS LIMIT GOES HERE
    })

    console.log("Here is the transactionId: " + transactionId);
    fcl.tx(transactionId).subscribe(res => {
      console.log(res);
      if (res.status === 0 || res.status === 1) {
        settxStatusGreeting('Pending...');
      } else if (res.status === 2) {
        settxStatusGreeting('Finalized...')
      } else if (res.status === 3) {
        settxStatusGreeting('Executed...');
      } else if (res.status === 4) {
        settxStatusGreeting('Sealed!');
        setTimeout(() => settxStatusGreeting('Run New Greeting Transaction'), 2000); // Incase of timeout
      }
    })
    await fcl.tx(transactionId).onceSealed();
    executeScript();
  }

  async function runNewNumberTransaction() {
    const transactionId = await fcl.mutate({
      cadence: `
      import SimpleTest from 0x6c0d53c676256e8c

      transaction(myNewNumber: Int) {

        prepare(signer: AuthAccount) {}

        execute {
          SimpleTest.updateNumber(newNumber: myNewNumber)
        }
      }
      `,
      args: (arg, t) => [
        arg(newNumber, t.Int)
      ], // ARGUMENTS GO IN HERE
      proposer: fcl.authz, // PROPOSER GOES HERE
      payer: fcl.authz, // PAYER GOES HERE
      authorizations: [fcl.authz], // AUTHORIZATIONS GO HERE
      limit: 999 // GAS LIMIT GOES HERE
    })

    console.log("Here is the transactionId: " + transactionId);
    fcl.tx(transactionId).subscribe(res => {
      console.log(res);
      if (res.status === 0 || res.status === 1) {
        settxStatusNumber('Pending...');
      } else if (res.status === 2) {
        settxStatusNumber('Finalized...')
      } else if (res.status === 3) {
        settxStatusNumber('Executed...');
      } else if (res.status === 4) {
        settxStatusNumber('Sealed!');
        setTimeout(() => settxStatusNumber('Run New Number Transaction'), 2000); // Incase of timeout
      }
    })
    await fcl.tx(transactionId).onceSealed();
    executeNewNumberReader();
  }

  async function executeScript() {
    const response = await fcl.query({
      cadence: `
      import HelloWorld from 0x8e56d24cc7f80589

      pub fun main(): String {
          return HelloWorld.greeting
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [] // ARGUMENTS GO IN HERE
    })

    console.log("Response from our script: " + response);
    setGreeting(response);
  }
  useEffect(() => {
    executeScript()
  }, [])

  async function executeNewNumberReader() {
    const response = await fcl.query({
      cadence: `
      import SimpleTest from 0x6c0d53c676256e8c

      pub fun main(): Int {
          return SimpleTest.number
      }
      `,
      args: (arg, t) => []
    })
    console.log("Response from the SimpleTest script: " + response);
    setNumber(response);
  }
  useEffect(() => {
    executeNewNumberReader()
  }, [])

  //function printGoodbye() {
  //console.log("Goats are pyschotic sheep!")
  //}

  return (
    <div className={styles.container}>
      <Head>
        <title>Emerald Dapp</title>
        <meta name="description" content="Created by Emerald Academy" />
        <link rel="icon" href="https://i.imgur.com/hvNtbgD.png" />
      </Head>

      <Nav />

      <div className={styles.welcome}>
        <h1 className={styles.title}>
          Welcome to my <a href="https://academy.ecdao.org" target="_blank">Emerald Dapp!</a>
        </h1>
        <p>This is a Dapp created by DarthNoobius.</p>
      </div>

      <main className={styles.main}>
        
        <div className={styles.flex}>
          <input onChange={(e) => setNewGreeting(e.target.value)} placeholder="Pizza is always the answer! Enter your salutations." />
          <button onClick={runNewGreetingTransaction}><span></span>{txStatusGreeting}</button>
        
          <input onChange={(e) => setnewNumber(e.target.value)} placeholder="8 is infinity in disguise. What is your number?" />
          <button onClick={runNewNumberTransaction}><span></span>{txStatusNumber}</button>
        </div>
      </main>
      <br></br>
      <div className={styles.welcome1}>
        <p>{greeting}</p>
        <p>{number}</p>
      </div>
    </div>
  )
}
```
