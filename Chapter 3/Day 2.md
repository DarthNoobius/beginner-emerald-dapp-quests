
# Quests
1. Explain why we wouldn't call changeGreeting in a script.   
changeGreeting is a function that changes the value store in the greeting variable. This therefore means it modifies the state of the contract. Scripts can only view the state of the contract therefore making changeGreeting uncallable. 

2. What does the AuthAccount mean in the prepare phase of the transaction?   
When a user signs a transaction, the AuthAccount uses this signature to access the data inside the account of the user such as the contracts and tokens needed to perform the transaction. Without it, the transaction cannot access the users account thus the transaction will fail.

3. What is the difference between the prepare phase and the execute phase in the transaction?  
The prepare phase gains access to the users account using AuthAccount while the execute phase calls the functions stored in the smart contract that changes the data stored on the blockchain. The prepare phase can also call functions but by convention, function calls are only done on the execute phase to make the code more readable. However, the execute phase cannot access the users account without a prepare phase.

4. This is the hardest quest so far, so if it takes you some time, do not worry! I can help you in the Discord if you have questions.

- Add two new things inside your contract:

  + A variable named myNumber that has type Int (set it to 0 when the contract is deployed)

  + A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber

- Add a script that reads myNumber from the contract

- Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%203/Images/Day%202%200x01%20account.png)
 0x01 account.png
 
![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%203/Images/Day%202%20script%20unchanged.png)
 Script unchanged.png
 
![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%203/Images/Day%202%20script%20changed.png)
 Script changed.png 
 
![](https://github.com/DarthNoobius/beginner-emerald-dapp-quests/blob/main/Chapter%203/Images/Day%202%20Transaction%20newNumber.png)
 Transaction newNumber.png  
