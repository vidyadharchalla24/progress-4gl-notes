### LOCKS AND TRANSACTIONS
- NO-LOCK : It is used when we would like to read or export the records from the database. It will only get the available details in database, It won't check if any user is trying to update the same field.
- EXCLUSIVE-LOCK : This is need to be used whenever we are going to update a record in database.
    -   Here if one user is already updating the record using exclusive lock and if other user tries to access the same using exclusive lock then he won't be able access it until other user completes his update operation.
- SHARE-LOCK : It is similar to Excusive-lock but the only difference is that multiple users can access the data (update and display) without any error message. 
    -   DisAdvantage is that when we try to display and update the same record in multiple sessions at the same time then it will go into an deadlock situation where one will wait for other session to complete its operation and other will wait for first one to complete its operation.
    ```
    FIND customer SHARE-LOCK WHERE customer.cust-num = 1 NO-UNDO.
    IF AVAILABLE customer THEN
    DO:
        DISPLAY customer.cust-num customer.name.
        PAUSE.
        UPDATE customer.name.
    END.
    /* Executing this same procedure in multiple sessions then we get Customer in use by user_name on CON:. wait or choose CANCEL to stop */
    ```
- NO-WAIT : 
- Bleeding Record Lock: 
```
DEFINE BUFFER bfcustomer FOR customer.
FIND FIRST bfcustomer WHERE bfcutomer.cust-num = 4 NO-LOCK NO-ERROR.
DISPLAY bfcutomer.cust-num bfcutomer.name.
UPDATE bfcutomer.name.

FIND FIRST customer WHERE cutomer.cust-num = 4 EXCLUSIVE-LOCK NO-ERROR.
DISPLAY cutomer.cust-num cutomer.name.
UPDATE cutomer.name.

UPDATE bfcustomer.name.

/*
For the no-lock if we try to update the value then it will give an error. If don't try to update the value of no-lock and then try to update the exclusive locks value then we won't get any error.
and after that we will be able to update the value with help of last statement.
*/
```