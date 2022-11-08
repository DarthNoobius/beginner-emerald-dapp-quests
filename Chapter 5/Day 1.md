# Quests
1. List all the possible transaction status codes and what each of them mean.
    - status code 0 : Unknown
    - status cod 1 : Transaction Pending
    - status code 2 : Finalized
    - status code 3 : Executed
    - status code 4 : Sealed
    - status code 5 : Expired

2. a. What does setTimeout do?
    It executes some code after a specified amount of time has elapsed.

2. b. How would we change our code if we wanted the txStatus variable to reset back to its original state after 5 seconds?  
    We would change the time parameter in the setTimeout function from 2000 to 5000  

3. What does the fcl.tx(transactionId).subscribe(res => {...}) function do?  
  This function gives us the value of the new status code of a transaction, which is stored in the res object, everytime the status of the transaction changes such as     from status 3(executed) to status 4 (sealed)
  
4. Make at least 3 changes to the styling of the application. It can be anything (part of this quest is being creative!). List the 3 changes and point them out in a        screenshot.  
  ![](https://github.com/DarthNoobius/emerald--dapp-quests/blob/master/Chapter%205/Images/Day%205%20webpage.png)
+ Buttons have a new hover graphic.
+ The greeting and number values are displayde at the bottom
+ The dapp has different colors
