# Account Management
## Project subject
- Accouunt Management Using SpringBoot

## Project Structure 
![image](https://user-images.githubusercontent.com/94863168/226207336-1629cda0-68b0-43ba-84df-4c980a5b62cc.png)


## Requirement
- If you get request for a transaction from an external system, you receive transaction information from a system that provides a transaction management function.

## ERD
![제목 없음](https://user-images.githubusercontent.com/94863168/226205791-4f4c9f43-487e-4a93-8e1f-df423c67e190.png)

## Tech Stack
Language : Java  
DBMS : H2  
Java Version : Java 8  
IDE : Intellij IDEA 2023.3.2 (Ultimate Edition)  
Test Program : Intellij IDEA 2023.3.2 (Ultimate Edition)  
ETC : Redis

## Implementation function (API)
Account
- Account creation
- Account closure
- Account Verification

Transaction
- Use Balance (Create Transaction)
- Cancellation of balance use (transaction cancellation)
- Transaction confirmation

## Function specification
USER
- User information management through H2 DB

Account
  - Account Creation API  
    - POST /account  
    - Parameters : User ID, Initial Balance
    - Policy : Failure response if there are no users or 10 accounts
    - Success response: User ID, account number, registration date
  - Account Closing API
    - DELETE /account
    - Parameters : User ID, Account
    - Policy : Policy: Failure response if user or account does not exist, if user ID and account owner are different, if account is already closed, if balance exists
    - Success response: User ID, account number, cancellation date and time
  - Account Verification API
    - GET /account?user_id={userId}
    - Parameter : User ID
    - Policy: Fail response if user does not exist
    - Success response: Response in the structure of List<account number, balance>
    
 Transaction
  - Balance Usage API
    - POST /transaction/use
    - Parameters : User ID, Account, Amount
    - Policy :
        - If there is no user
        - User ID and account holder are different
        - If your account is already closed
        - If the transaction amount is greater than the balance,
        - Failure response if the transaction amount is too small or too large
        - If another transaction request comes while a transaction (use, cancellation) is in progress in the account, the corresponding transaction must be prevented from being processed incorrectly at the same time.
    - Success response: account number, transaction result code (success/failure), Transaction ID, transaction amount, transaction date and time
    
    
  - Balance Redemption API
    - POST /transaction/cancel
    - Parameters: transaction ID, cancellation request amount.
    - Policy :
      - If there is no transaction corresponding to the transaction ID
      - Transactions older than 1 year cannot be canceled
    - Success response: account number, transaction result code (success/failure), transaction ID, transaction amount, transaction date and time.
  
  - Transaction Confirmation API
    - GET /transaction/{transactionId}
    - Parameter: Transaction ID
    - Policy: Failure response if there is no transaction with the corresponding transaction ID
    - Success response: account number, transaction type (balance use, balance use cancellation), transaction result code (success/failure), transaction ID, transaction amount, transaction date and time
    - Failed transactions (redemption/cancellation) also allow transactions to be confirmed.
    
    
  
    
    
    
