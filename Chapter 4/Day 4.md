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
  
  async function runTransaction() {
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

      <main className={styles.main}>
        <h1 className={styles.title}>
          Welcome to my <a href="https://github.com/DarthNoobius/beginner-emerald-dapp" target="_blank">Emerald DApp!</a>
        </h1>

        <p className={styles.p}>
        There is no such thing as too much garlic
        </p>

        <div className={styles.flex}>
        <button onClick={runTransaction}>Run Transaction</button>
        <input onChange={(e) => setNewGreeting(e.target.value)} placeholder="Pizza is always the answer!" />
        </div>

        <div className={styles.flex}>
        <button onClick={runNewNumberTransaction}>Run Transaction</button>
        <input onChange={(e) => setnewNumber(e.target.value)} placeholder="Enter number" />
        </div>
        
        <p> {greeting}</p>
        <p> {number}</p>

      </main>
    </div>
  )
}
```
