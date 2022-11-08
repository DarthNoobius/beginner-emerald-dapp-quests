# Quests
1. How did we get the address of the user? Please explain in words and then in code.  
   Using a button log in button that links to the `handleAuthentication` function. The `handleAuthentication` function logs in the user by having them select a wallet      that is provided by the `config.js file` and stores it the `user` variable.
    ```javascript
    button onClick={handleAuthentication}>Log In</button>
    ```
2. What do `fcl.authenticate` and `fcl.unauthenticate` do?
   - fcl.authenticate: logs the user in
   - fcl.unauthenticate: logs the user out

3. Why did we make a config.js file? What does it do?
   This file enables the dapp to be able to connect to the Flow blockchain by showing it whether to connect to mainnet or testnet and the wallets that are available. It    does this by using the Flow Client Library (FCL)

4. What does our useEffect do?   
    useEffect is a function that runs every time something happens. That "something" comes from what is put inside the [] brackets.

5. How do we import FCL?
    ```javascript
    import * as fcl from "@onflow/fcl";
    ```

6. What does fcl.currentUser.subscribe(setUser); do?  
   Sets the `user` variable to the currentUser and make sure this value is reatianed even when the page is refreshed.
