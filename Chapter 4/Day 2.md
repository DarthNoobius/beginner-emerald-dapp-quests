# Quest 
1. Instead of console logging the result after the script executes, I want you to:
- Make a new variable named `greeting` using `useState`
- Set the `greeting` variable to the `response` of the script call
- Create a `<p>` tag after the `<div className={styles.flex}>` tag
- Put the greeting variable inside of that <p> tag. This will make the result of your script show on your webpage! It should look something like this:

 ![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%204/Images/Day%202%20greeting%20console.png)
 greeting console.png
 ```javascript
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import Nav from "../components/Nav.jsx"
import { useState, useEffect } from 'react';
import * as fcl from "@onflow/fcl"


export default function Home() {
  const [newGreeting, setNewGreeting] = useState('');
  const [greeting, setGreeting] = useState('')
  
  function runTransaction() {
    console.log("Well done steak is better than medium rare")
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

  function printGoodbye() {
    console.log("Goats are pyschotic sheep!")
  }

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

        <p> {greeting}</p>

      </main>
    </div>
  )
}
```
2a. I deployed a contract called SimpleTest to an account with an address of 0x6c0d53c676256e8c. I want you to make a button that, when clicked, executes a script to read the number variable from that contract. If you're curious, you can see the contract here: https://flow-view-source.com/testnet/account/0x6c0d53c676256e8c/contract/SimpleTest
+ Submit all the code you used to call the script, and the result of the script.
    ![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%204/Images/Day%202%20SimpleTest%20button.png)
    SimpleTest button.png
    
 ```javascript
 import Head from 'next/head'
 import styles from '../styles/Home.module.css'
 import Nav from "../components/Nav.jsx"
 import { useState, useEffect } from 'react';
 import * as fcl from "@onflow/fcl"


 export default function Home() {
 const [newGreeting, setNewGreeting] = useState('');
 const [greeting, setGreeting] = useState('');
 const [number, setNumber] = useState('');

 function runTransaction() {
     console.log("Well done steak is better than medium rare")
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

 async function numberReader() {
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

 function printGoodbye() {
     console.log("Goats are pyschotic sheep!")
 }

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
         <button onClick={numberReader}>What's the number?</button>
         </div>

         <p> {greeting}</p>
         <p> {number}</p>



     </main>
     </div>
 )
 }
 ```
2b. Then, I want you to remove the button, and make the script execute every time the page refreshes.  
    ![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%204/Images/Day%202%20Simpletest%20without%20button.png)
    Simpletest without button.png  
```javascript
    import Head from 'next/head'
    import styles from '../styles/Home.module.css'
    import Nav from "../components/Nav.jsx"
    import { useState, useEffect } from 'react';
    import * as fcl from "@onflow/fcl"


    export default function Home() {
    const [newGreeting, setNewGreeting] = useState('');
    const [greeting, setGreeting] = useState('');
    const [number, setNumber] = useState('');
    
    function runTransaction() {
        console.log("Well done steak is better than medium rare")
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
    
        //console.log("Response from our script: " + response);
        setGreeting(response);
    }
    useEffect(() => {
        executeScript()
    }, [])

    async function numberReader() {
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
        numberReader()
    }, [])

    function printGoodbye() {
        console.log("Goats are pyschotic sheep!")
    }

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
            
            </div>
            
            <p> {greeting}</p>
            <p> {number}</p>

        </main>
        </div>
    )
    }
```  
Submit all the code you used to do this.
 
